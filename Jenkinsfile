pipeline {
    agent any

    tools {
        maven 'maven3' // Ensure "maven3" matches your Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout From Git') {
            steps {
                git branch: 'prod', url: 'https://github.com/Habizanoor/enahanced-petclinc-springboot.git'
            }
        }

        stage('Maven Compile') {
            steps {
                echo 'This is Maven Compile stage'
                sh 'mvn compile'
            }
        }

        stage('Maven Test') {
            steps {
                echo 'This is Maven Test stage'
                sh 'mvn test'
            }
        }
        stage('MFile System Scan By Trivy') {
            steps {
                echo 'Trivy scanning started'
                sh 'trivy fs --format table --output trivy-report.txt --severity HIGH,CRITICAL .'


            }
        }
    }
}

