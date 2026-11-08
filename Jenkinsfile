// Check GitHub every 2 minutes for new commits
properties([pipelineTriggers([pollSCM('H/2 * * * *')])])

node {
    // Remove the container of the previous deployment.
    stage('Preparation') {
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
}
