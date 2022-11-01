node {
    withDockerContainer(image: 'python:2-alpine'){
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    withDockerContainer(image: 'qnib/pytest'){
        stage('Test') {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } finally {
                junit 'test-reports/results.xml'
            }
        } 
    }
    withDockerContainer(image: 'cdrx/pyinstaller-linux:python2'){
        stage('Deploy') {
            sh 'pyinstaller --onefile sources/add2vals.py'
            archiveArtifacts 'dist/add2vals'
        } 
    }
}
