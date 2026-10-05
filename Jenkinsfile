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
}
