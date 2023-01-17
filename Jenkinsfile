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
    stage('Deliver') {
        docker.image('cdrx/pyinstaller-linux:python2').inside  {
                sh "docker -v"             
        }
    }
    
}
