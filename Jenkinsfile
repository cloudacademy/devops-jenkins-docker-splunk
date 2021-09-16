//START-OF-SCRIPT
node {
    def SPLUNK_HOSTNAME='splunk'
    def DOCKER_HOME = tool name: 'docker-latest'
    def GRADLE_HOME = tool name: 'gradle-4.10.2', type: 'hudson.plugins.gradle.GradleInstallation'
    def REPO_URL = 'https://github.com/cloudacademy/devops-webapp.git'
    def DOCKERHUB_REPO = 'cloudacademydevops/webapp'

    stage('Clone') {        
        git url: REPO_URL
    }

    stage('Build') {
        sh "${GRADLE_HOME}/bin/gradle build"
        sh "ls -la build/libs/*.war"
        sh "echo ====================="
        sh "cp build/libs/*.war docker/webapp.war"
        sh "pwd"
        sh "ls -la ./docker"
    }

    stage ('Docker Build') {
        docker.withTool('docker-latest') {
            sh "printenv"
            sh "pwd"
            sh "ls -la"
            def image1 = docker.build("${DOCKERHUB_REPO}:${BUILD_NUMBER}", "./docker")
            def image2 = docker.build("${DOCKERHUB_REPO}:latest", "./docker")
        }
    }

    /*
    stage ('Docker Push') {
        docker.withTool('docker-latest') {
            withCredentials([usernamePassword(credentialsId: 'dockerhub',
                             usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh "docker login -u ${USERNAME} -p ${PASSWORD} https://index.docker.io/v1/"
                sh "docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}"
                sh "docker push ${DOCKERHUB_REPO}:latest"
            }            
        }
    }
    */
}
//END-OF-SCRIPT
