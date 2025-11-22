pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Clonage du dépôt GitHub...'
                checkout scm
            }
        }

        stage('Build & Tests') {
            steps {
                echo '⚙️ Compilation et exécution des tests...'
                sh 'mvn clean verify'
            }
        }

        stage('Package') {
            steps {
                echo ' Génération du fichier JAR...'
                sh 'mvn package'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo ' Archivage du JAR généré...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            echo '📊 Publication des résultats de tests...'
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }

        success {
            echo ' Build terminé avec succès ! Le fichier JAR est disponible dans target/'
        }

        failure {
            echo ' Le build a échoué.'
        }
    }
}
