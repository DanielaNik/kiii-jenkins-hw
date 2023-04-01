node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
        if (env.BRANCH_NAME == 'dev' || env.CHANGE_TARGET == 'dev') {
            app = docker.build("danielanik/kiii-jenkins-hw")
        } else {
            echo "Skipping image build for branch ${env.BRANCH_NAME}"
        }
    }
    stage('Push image') {
        if ((env.BRANCH_NAME == 'dev' || env.CHANGE_TARGET == 'dev') && app) {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                app.push("${env.BRANCH_NAME}-latest")
                // signal the orchestrator that there is a new version
            }
        } else {
            echo "Skipping image push for branch ${env.BRANCH_NAME}"
        }
    }
}
