 2.1 Project Structure (Maven Web App)

Your GitHub repository should be structured like:

maven-web-project/
  ├── src/main/webapp/...
  ├── pom.xml     (packaging: war)
  └── Jenkinsfile


After building, Maven will generate:

target/yourapp.war

 2.2 Install & Configure Tomcat (Linux – Ubuntu/Debian)

Install Tomcat 9:

sudo apt update
sudo apt install tomcat9 tomcat9-admin -y


Default deployment directory:

/var/lib/tomcat9/webapps/


Check Tomcat is running:

http://<server-ip>:8080

 2.3 Jenkinsfile (3-Stage Pipeline for Web App)

This Jenkinsfile performs:

Checkout code

Build WAR

Deploy to Tomcat

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

✔ Notes

TOMCAT_WEBAPPS points to Tomcat deployment folder.

WAR gets copied as:

cp target/*.war /var/lib/tomcat9/webapps/myapp.war


Tomcat restarts to load the new deployment:

systemctl restart tomcat9


Some lab systems allow passwordless sudo.

Commit and push the Jenkinsfile:

git add Jenkinsfile
git commit -m "Add Jenkinsfile for web deploy pipeline"
git push origin main

 2.4 Create Jenkins Pipeline Job

Open Jenkins → New Item

Name: web-project-pipeline

Type: Pipeline

Under Pipeline section:

Definition: Pipeline script from SCM
SCM: Git
Repository: https://github.com/your-username/maven-web-project.git
Branch: */main
Script Path: Jenkinsfile


Click Save.

 2.5 Run the Pipeline

Go to the job → Build Now

Expected Output:

✔ Checkout Stage

GitHub clone success

✔ Build WAR Stage

[INFO] BUILD SUCCESS


✔ Deploy to Tomcat Stage

cp target/yourapp.war /var/lib/tomcat9/webapps/myapp.war
systemctl restart tomcat9


✔ Final

Deployment done. Check http://<server-ip>:8080/myapp/
Finished: SUCCESS

 2.6 Verify Deployment in Browser

Open:

http://localhost:8080/myapp/


Or if using a lab machine:

http://<server-ip>:8080/myapp/


You should now see your web application's homepage (e.g., index.jsp).