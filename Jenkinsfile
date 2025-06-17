pipeline {
    agent {label 'linux-agent'}
    stages {
        stage('Chekout From Git') {
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
    }
}
