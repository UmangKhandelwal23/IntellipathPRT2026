pipeline {
    agent { label 'Slave01' }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker')
    }

    stages {

        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/UmangKhandelwal23/IntellipathPRT2026'
            }
        }

        stage('docker build') {
            steps {
                sh 'sudo docker build -t umangkhandelwal/prtapril2026:v1 .'
            }
        }

        stage('Docker Push') {
            steps {
                sh '''
                echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                docker push umangkhandelwal/prtapril2026:v1
                '''
            }
        }

        stage('Create Namespace') {
            steps {
                sh 'kubectl get ns employee-portal || kubectl create ns employee-portal'
            }
        }

        stage('K8 Deploy') {
            steps {
                sh '''
                kubectl apply -f deployment.yml -n employee-portal
                kubectl apply -f service.yml -n employee-portal
                '''
            }
        }
    }
}
