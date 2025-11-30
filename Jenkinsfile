pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Dr-Venom29/timepass.git'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling Java file...'
                bat 'javac Hello.java'
            }
        }

        stage('Run') {
            steps {
                echo 'Running program...'
                bat 'java Hello'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS - Java program compiled and executed!'
        }
    }
}
