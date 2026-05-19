pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        app_version = "1.1.2"
    }
    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    app_version = packageJson.version
                    echo "Read version from package.json: ${app_version}"
                }
            }
        }
    }

}