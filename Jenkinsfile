pipeline {
    agent any

    environment {
        IMAGE = "sihyuk11/jenkins:v4"
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-id',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $IMAGE'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    kubectl apply -f /DATA/jenkins/v3/deploy.yml &&
                    kubectl apply -f /DATA/jenkins/v3/svc.yml &&
                    kubectl rollout status deployment/jenkins-v3
                    "
                '''
            }
        }

    }
}

