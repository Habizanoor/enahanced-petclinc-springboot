pipeline {
    agent { label 'linux-agent'}
    tools {
        maven 'maven'
    }
    stages {
        stage('Checkout From Git') { 
            steps {
               git branch: 'main', url: 'https://github.com/Habizanoor/enahanced-petclinc-springboot.git'

            }
        }
        stage('Maven Compile') { 
            steps {
                echo 'This Maven Compile Stage'
                sh 'mvn compile'
            }
        }
        stage('Maven Test') { 
            steps {
                echo 'This Maven Test Stage'
                sh 'mvn test'
            }
        }
    }
}
