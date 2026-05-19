pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        app_version = "1.1.2"
        ACC_ID = "134163283084"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"

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
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Image') {
            steps {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds'){
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS 
                            --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${app_version} .

                            docker images

                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${app_version}
                            
                        """
                    }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }

}