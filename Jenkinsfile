pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "wafagharbi02/student-app"
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('GIT') {
            steps {
                echo "Getting Project from Git"
                git branch: 'main', url: 'https://github.com/wafagharbi02/DevOpsProject.git'
            }
        }

        stage('MVN CLEAN') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('MVN COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('MVN PACKAGE') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('DOCKER BUILD & PUSH') {
            steps {
                script {
                    def image = docker.build("${DOCKER_IMAGE}:latest")
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        image.push()
                    }
                }
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar -DskipTests'
                }
            }
        }

        stage('Déploiement Kubernetes') {
            steps {
                echo "Déploiement de l'application sur Kubernetes"
                sh 'kubectl apply -f kubernetes/mysql-deployment.yaml'
                sh 'kubectl rollout status deployment/mysql -n devops --timeout=300s'

                sh 'kubectl apply -f kubernetes/spring-deployment.yaml'
                sh 'kubectl rollout status deployment/stationskicontainer -n devops --timeout=300s'

                sh 'kubectl get all -n devops'
            }
        }
    }
}