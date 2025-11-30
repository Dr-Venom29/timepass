pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
        jdk 'Java-17'
    }

    environment {
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/maven-web-project.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // assuming WAR is target/app.war
                sh """
                sudo cp target/*.war ${TOMCAT_WEBAPPS}/myapp.war
                sudo systemctl restart tomcat9
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment done. Check http://<server-ip>:8080/myapp/'
        }
    }
}
