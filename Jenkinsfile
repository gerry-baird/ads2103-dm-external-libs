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
                     sh '''
                       echo "Running ADS Deployment using REST"
                       echo "Build Number ${BUILD_NUMBER}"
                       curl -k -X 'POST' \
                       "https://cpd-cp4ba.mycluster-eu-gb-1-cx2-16x-4d2c0e6e364e1cb6bda1360a996d18f0-0000.eu-gb.containers.appdomain.cloud/ads/runtime/api/v1/deploymentSpaces/embedded/decisions/ads-external-lib-devops-${BUILD_NUMBER}/archive" \
                       -H 'accept: */*' \
                       -H 'Authorization: ZenApiKey ${ZenApiKey}' \
                       -H 'Content-Type: application/octet-stream' \
                       --data-binary @/var/lib/jenkins/.m2/repository/cpadmin/gb_external_library/utils_test/utils_TestDecisionService/1.0.0-SNAPSHOT/utils_TestDecisionService-1.0.0-SNAPSHOT.jar
                     '''
                }

            }
        }
    }
}