pipeline {
    agent any
    
    triggers {
        // Example triggers (you can enable one at a time when testing)
        pollSCM('* * * * *')          // Check for changes in GitHub every minute
        cron('H 20 * * 1-5')          // Scheduled job: run at 8:00 PM, Mon–Fri
        // githubPush()               // Trigger when GitHub webhook is received
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}
