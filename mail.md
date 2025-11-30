 1. Jenkinsfile

Place this in your Jenkins pipeline job under:

Jenkins → New Item → Pipeline → Pipeline Script

Or if using GitHub:

Place Jenkinsfile in the project root:

/project-root/
    └── Jenkinsfile


Jenkinsfile used:

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building..."
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: Job ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: "Build succeeded.\nSee details at ${env.BUILD_URL}",
                to: 'smartdocq@gmail.com'
            )
        }
        failure {
            emailext (
                subject: "FAILURE: Job ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: "Build failed.\nSee details at ${env.BUILD_URL}",
                to: 'smartdocq@gmail.com'
            )
        }
    }
}

 2. Where to Put Email Configuration (SMTP Settings)

These SMTP settings are NOT placed in the Jenkinsfile.
They go into:

Manage Jenkins → Configure System → E-mail Notification (and Extended E-mail Notification)

Enable:

SMTP Server: smtp.gmail.com
Port: 465
Use SMTP Authentication: ✔
Use SSL: ✔
User: smartdocq@gmail.com
Password: <your Google App Password>
Reply-To Address: smartdocq@gmail.com

3. Additional SMTP Properties (Important)

These properties belong inside Advanced Email Properties under:

Manage Jenkins → Configure System → Extended E-mail Notification → Advanced Email Properties

Paste this exactly:

mail.smtp.auth=true
mail.smtp.starttls.enable=true
mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory


They ensure Gmail SSL/TLS works with Jenkins.

 4. Required Jenkins Plugins

Make sure these are installed:

✔ Email Extension Plugin (emailext)

✔ Mailer Plugin

Without these, Jenkinsfile's emailext{} will not work.

 5. Running the Pipeline

After saving the Jenkinsfile:

Click Build Now

Watch console:

Sending email to: smartdocq@gmail.com


If no errors → success.

The email will show:

Build SUCCESS or FAILURE

Build URL

Jenkins job name

Build number