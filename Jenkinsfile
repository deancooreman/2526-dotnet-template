// Check GitHub every 2 minutes for new commits
properties([pipelineTriggers([pollSCM('H/2 * * * *')])])

node {
    // Check out the code and remove the container of the previous deployment.
    stage('Preparation') {
        // Check out the repo, so Jenkins knows which commit was built (needed for Poll SCM)
        checkout scm

        catchError(buildResult: 'SUCCESS') {
            sh 'docker stop riserunning'   // stop the running container
            sh 'docker rm riserunning'     // delete it, so the name is free again
        }
    }
    // Start the freestyle job BuildRiseApp.
    stage('Build') {
        build 'BuildRiseApp'
    }
    // Start the freestyle job TestRiseApp.
    stage('Test') {
        build 'TestRiseApp'
    }
    // Deploy the same app to production
    stage('Deploy to production') {
        sshagent(['dmz010-deploy']) {
            // Trust the host key of dmz010
            sh 'mkdir -p ~/.ssh && ssh-keyscan 192.0.2.10 >> ~/.ssh/known_hosts'
            // RISEAPP_PUBLISH: only listen on localhost, nginx is the reverse proxy
            withEnv(['DOCKER_HOST=ssh://jenkins@192.0.2.10', 'RISEAPP_PUBLISH=127.0.0.1:5001']) {
                sh 'docker rm -f riserunning || true'
                sh 'rm -rf tempdir'
                sh 'bash rise-app.sh'
            }
        }
    }
}
