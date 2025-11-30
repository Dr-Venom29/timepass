 1. Prerequisites

Before starting, make sure you have:

✔ Jenkins Installed (running on localhost:9000)
✔ Git Installed
✔ GitHub Repository
✔ ngrok Installed (to expose Jenkins to GitHub)
 2. Start ngrok to Expose Jenkins

Since Jenkins runs locally, GitHub cannot reach it directly.
We use ngrok to create a public URL.

Run this command:
ngrok http 9000


Your output will look like:

Forwarding https://abcd-12-34-56.ngrok-free.app -> http://localhost:9000

 The URL you will use for webhook:
https://abcd-12-34-56.ngrok-free.app/github-webhook/

 3. Configure Jenkins for GitHub Webhook

Go to:

Jenkins → Your Job → Configure → Build Triggers

Enable:

 GitHub hook trigger for GITScm polling

Save the job.

 4. Add Jenkinsfile to Your GitHub Repo

Inside your repository, create a file:

Jenkinsfile


Paste this minimal working pipeline:

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
            echo "Build SUCCESS – Webhook worked!"
        }
        failure {
            echo "Build FAILED!"
        }
    }
}


Commit & push:

git add .
git commit -m "add Jenkinsfile"
git push origin main

 5. Configure Webhook in GitHub

Go to:

GitHub → Your Repo → Settings → Webhooks → Add Webhook

Fill details:
Field	Value
Payload URL	https://<ngrok-url>/github-webhook/
Content type	application/json
Secret	leave empty (for exam)
Trigger	Just the push event

Click "Add Webhook"
Status should show green ✔

 6. Test Webhook Auto Trigger

Make any small change in your repo.
Example edit README.md or any file.

Then:

git add .
git commit -m "webhook test"
git push origin main

 7. Verify in Jenkins

Open Jenkins → Your Job
You will see a new build triggered automatically:

Started by GitHub push by rohit
Obtained Jenkinsfile from git
Commit message: "webhook test"


Pipeline output:

Building...
Build SUCCESS – Webhook worked!
Finished: SUCCESS


This confirms:

✔ GitHub triggered Jenkins
✔ Jenkins pulled latest code
✔ Jenkinsfile executed
✔ Build completed automatically