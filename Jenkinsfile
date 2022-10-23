node {
    stage('Build') {
        withDockerContainer(image: 'python:2-alpine'){
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        withDockerContainer(image: 'qnib/pytest'){
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } finally {
                junit 'test-reports/results.xml'
            }
        } 
    }
    stage('Deliver') {
        withDockerContainer(image: 'cdrx/pyinstaller-linux:python2'){
            try {
                sh 'pyinstaller --onefile sources/add2vals.py'
            } finally {
                archiveArtifacts 'dist/add2vals'
            }
        }
    }
}
