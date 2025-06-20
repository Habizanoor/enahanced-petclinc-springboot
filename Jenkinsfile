pipeline {
    agent any

    tools {
        maven 'maven3' // Ensure "maven3" matches your Jenkins Global Tool Configuration
    }

    environment{
        IMAGE_NAME = "spring-boot"
        IMAGE_TAG = "latest"
        ACR_NAME = "dockerregisry"
        ACR_LOGIN_SERVER = "${ACR_NAME}.azurecr.io"
        FULL_IMAGE_NAME = "${ACR_LOGIN_SERVER}/${IMAGE_NAME}:${IMAGE_TAG}" //example : dockerregisry.azurecr.io/spring-boot:latest
        TENANT_ID = "105943eb-0807-487d-acb3-e7c34ef4de26"
    }
    stages {
        stage('Checkout From Git') {
            steps {
                git branch: 'prod', url: 'https://github.com/Habizanoor/enahanced-petclinc-springboot.git'
            }
        }

        //stage('Maven Compile') {
        //    steps {
        //        echo 'This is Maven Compile stage'
        //        sh 'mvn compile'
        //    }
        //}

        //stage('Maven Test') {
        //    steps {
        //        echo 'This is Maven Test stage'
        //        sh 'mvn test'
        //    }
        //}
        stage('File System Scan By Trivy') {
            steps {
                echo 'Trivy scanning started'
                sh 'trivy fs --format table --output trivy-report.txt --severity HIGH,CRITICAL .'


            }
        }
        //stage('Sonar Analysis') {
        //    environment{
        //        SCANNER_HOME = tool 'Sonar-scanner'
        //    }
        //    steps {
        //        withSonarQubeEnv('sonerserver'){
        //            sh '''
        //                $SCANNER_HOME/bin/sonar-scanner \
        //                -Dsonar.organization=HabizaNoorHussain \
        //                -Dsonar.projectName=enahanced-petclinc-springboot \
        //                -Dsonar.projectKey=Habizanoor_enahanced-petclinc-springboot \
        //                -Dsonar.java.binaries=. \
        //                -Dsonar.exclusions=**/trivy-report.txt
        //                
        //            '''
        //        }
        //    }
        //}

        //stage('Sonar Quality Gate') {
        //    steps {
        //        steps {
        //            timeout(time: 1, unit: 'MINUTES') {
        //            waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
        //            }
        //        }
        //    }
        //}
         stage('Maven Package') { 
            steps {
                echo 'This Maven Package Stage'
                sh 'mvn package'
            }
        }
        stage('Docker Build') { 
            steps {
                script{
                    echo 'Creating Docker Image'
                        docker.build("$IMAGE_NAME:$IMAGE_TAG")
                }
                
            }
        }
        stage('Azure Login to ACR') { 
            steps {
                withCredentials([usernamePassword(credentialsId: 'azurespn', usernameVariable: 'AZURE_USERNAME', passwordVariable: 'AZURE_PASSWORD')])
                    
                    script{
                        echo 'LOGIN TO Azure Container registry'
                        sh '''
                        az login --service-principal -u $AZURE_USERNAME -p $AZURE_PASSWORD --tenant $TENANT_ID
                        az acr login --name $ACR_NAME
                        '''
                }
            }
        }
    }
}
