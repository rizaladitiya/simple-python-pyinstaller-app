node {
	checkout scm
    stage('Build') {
        docker.image('python:2-alpine').inside {
            'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch (Exception e) {
                echo 'Error: ' + e.toString()
            } finally {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
    withEnv([
              'VOLUME=$(pwd)/sources:/src',
               'IMAGE=cdrx/pyinstaller-linux:python2']).inside {
        stage('Deliver') {
            sh 'printenv'
            docker.image('python:2-alpine') {
            	dir(path: env.BUILD_ID) { 
                    sh "pip install pyinstaller && pyinstaller sources/add2vals.py" 
                }
        	}
        }
    }
}
