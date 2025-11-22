pipeline {
    agent any

    tools {
        maven 'M2_HOME'   
        jdk   'JAVA_HOME'  
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Clonage du dépôt GitHub...'
                checkout scm
            }
        }

        stage('Build & Tests') {
            steps {
                echo 'Compilation et exécution des tests...'
                sh 'mvn clean verify'
            }
        }

        stage('Package') {
            steps {
                echo 'Génération du fichier JAR...'
                sh 'mvn package'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archivage du JAR généré...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, onlyIfSuccessful: true
            }
        }
    }

    post {
        always {
            echo 'Publication des rapports de tests...'
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
        success {
            echo 'BUILD RÉUSSI ! Le JAR est archivé et disponible dans Jenkins.'
        }
        failure {
            echo 'ÉCHEC DU BUILD. Vérifiez les logs ci-dessus.'
        }
    }
}
