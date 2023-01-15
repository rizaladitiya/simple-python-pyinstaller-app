node {
	checkout scm
    stage('Build') {
        docker.image('python:3.10.7-alpine').inside {
            'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
    }
    stage('Deliver') {
    	docker.image('cdrx/pyinstaller-linux:python2').inside("--entrypoint=''")  {
            sh 'pyinstaller --onefile sources/add2vals.py'
        }
    }
}
