pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code GitHub...'
                checkout scm
            }
        }
        stage('Compile & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, allowEmptyArchive: false
            }
        }
    }

    post {
        always {
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
        success {
            echo 'Build réussi ! Le .jar est généré dans target/'
        }
        failure {
            echo 'Échec du build'
        }
    }
}
