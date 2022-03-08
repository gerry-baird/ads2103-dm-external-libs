pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                        echo "PATH = ${PATH}"
                        echo "M2_HOME = ${M2_HOME}"
                        echo "Working Directory "
                        echo $PWD
                '''
            }
         }
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'mvn -X -s maven-settings.xml clean install -f Utils_Test/pom.xml'
            }
        }
        stage('Test') {
            steps {
                echo 'Skipping Tests..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                withCredentials([string(credentialsId: 'ZenApiKey-Dev', variable: 'ZenApiKey')]) {
                     echo 'Skipping Deploy..'
                }

            }
        }
    }
}