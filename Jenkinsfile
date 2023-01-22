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
                input message: 'Lanjutkan ke tahap Deploy? (Click "Proceed" to continue)'
            }
        }
    }
    stage('Deliver') {
        docker.image('six8/pyinstaller-alpine-linux-amd64:alpine-3.12-python-2.7-pyinstaller-v3.4').inside("--entrypoint=''")  {
        	try {
            		sh "pyinstaller -F sources/add2vals.py"           
            	} catch (Exception e) {
                echo 'Error: ' + e.toString()
            } finally {
                always {
                    archiveArtifacts 'dist/add2vals'
                    sh "rm -rf build dist"
                    sleep 60
                }
            }
        }
    }
    
}
