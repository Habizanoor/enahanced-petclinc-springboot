pipeline {
    agent any

    tools {
        maven 'maven3' // Ensure "maven3" matches your Jenkins Global Tool Configuration
    }

    environment{
        IMAGE_NAME = "spring-boot"
        IMAGE_TAG = "latest"
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
        //            waitForQualityGate abortPipeline: true, credentialsId: 'sonar-new'
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

    }
}
