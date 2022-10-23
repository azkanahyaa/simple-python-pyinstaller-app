node {
    stage('Build') {
        withDockerContainer(image: 'python:2-alpine'){
            sh 'pwd /sources'
            sh 'python -m py_compile ./sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        try {
            withDockerContainer(image: 'qnib/pytest'){
                sh 'py.test --verbose --junit-xml test-reports/results.xml ./sources/test_calc.py'
            }
        } finally {
            junit 'test-reports/results.xml'
        }
    }
    stage('Deliver') {
        try {
            withDockerContainer(image: 'cdrx/pyinstaller-linux:python2'){
                sh 'pyinstaller --onefile ./sources/add2vals.py'
            }
        } finally {
            archiveArtifacts 'dist/add2vals'
        }
    }
}
