Jenkins Triggers Demo
This document demonstrates different ways to trigger Jenkins jobs using a Jenkinsfile stored in a GitHub repository. It covers Git Webhooks, Poll SCM, Scheduled jobs, Remote triggers, and Build after other projects.
Popular Triggers in Jenkins
1. Git Webhook
• Jenkins job is triggered whenever there is a commit (or other events) in GitHub.
• Steps:
  1. Copy Jenkins URL (e.g. http://<jenkins-ip>:8080/github-webhook/).
  2. In GitHub repo → Settings → Webhooks → Add webhook.
  3. Paste the URL, set Content type = application/json.
  4. Select events (default: push event).
  5. In Jenkins job → Configure → Build Triggers → GitHub hook trigger for GITScm polling.
• Test: Commit & push a file, the job should trigger automatically.
2. Poll SCM
• Jenkins checks the Git repository periodically for changes.
• Opposite of webhook (Jenkins pulls instead of GitHub pushing).
• Configure in job → Build Triggers → Poll SCM.
• Use cron syntax (5 fields: minute, hour, day of month, month, day of week).
  Example:
    * * * * *     → every minute
    H/5 * * * *   → every 5 minutes
• Test: Make a commit; Jenkins will detect it during the next poll and trigger the job.
3. Scheduled Job (Build Periodically)
• Run jobs at specific times, regardless of commits.
• Same cron syntax as Poll SCM.
• Example: Run every weekday at 8:30 PM → 30 20 * * 1-5
• Useful for nightly builds, backups, or reports.
4. Remote Trigger
• Trigger Jenkins jobs externally using an API call.
• Steps:
  1. In job → Configure → Build Triggers → Trigger builds remotely.
  2. Define an authentication token (e.g. mybuildtoken).
  3. Generate an API token for Jenkins user.
  4. Generate CRUMB using wget command.
  5. Use curl command to trigger the job with username, token, and CRUMB.
• This allows jobs to be triggered from scripts, Ansible, or any external tool.
5. Build After Other Projects
• Chain jobs together: Job B starts when Job A completes.
• Steps:
  1. Create two jobs (build-job and test-job).
  2. In test-job → Build Triggers → Build after other projects are built → enter build-job.
• Test: Run build-job. When it finishes, test-job is triggered automatically.
Setup Steps
1. Create GitHub Repository:
   git init jenkinstriggers
   cd jenkinstriggers

2. Set Up SSH Authentication:
   ssh-keygen
   - Add public key to GitHub → Settings → SSH and GPG Keys.
   - Use private key in Jenkins credentials.

3. Add Jenkinsfile:
   pipeline {
       agent any
       stages {
           stage('Build') {
               steps {
                   echo 'Hello from Jenkins pipeline!'
               }
           }
       }
   }

4. Push to GitHub:
   git add Jenkinsfile
   git commit -m "Add simple Jenkinsfile"
   git push origin master

5. Configure Jenkins Job:
   - Pipeline script from SCM.
   - SCM = Git, use SSH repo URL.
   - Add SSH key credentials.
   - Path = Jenkinsfile.
Troubleshooting
• Host key verification failed → Go to Manage Jenkins → Configure Global Security → Git Host Key Verification → Accept first connection.
• Webhook not delivered →
  - Ensure Jenkins is accessible on port 8080 from the internet.
  - Security Group allows inbound 8080.
  - URL and trailing slash are correct (/github-webhook/).
  - Content type is JSON.
Summary
This repo demonstrates:
- Git Webhooks → Trigger on commits.
- Poll SCM → Jenkins checks for changes.
- Scheduled jobs → Run at fixed times.
- Remote triggers → Run from scripts / API calls.
- Build after other projects → Job chaining.

With these triggers, you can build powerful and flexible CI/CD pipelines in Jenkins 🚀
