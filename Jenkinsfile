node {
    def VERSION = '10-2.4'
    def VARIANT = 'alpine'
    def DOCKER_IMAGE = 'dvitali/postgis'
    
    stage('Checkout') {
        checkout scm
    }

    stage('Build Image'){
        sh "docker build --pull --no-cache -t ${DOCKER_IMAGE}:${VERSION}-${VARIANT} ${VERSION}/${VARIANT}"
    }

    stage('Push Image') {
        withCredentials([usernamePassword(
            credentialsId: "docker-hub-dvitali",
            usernameVariable: "USER",
            passwordVariable: "PASS"
        )]) {
            sh "docker login -u $USER -p $PASS"
        }

        sh "docker tag ${DOCKER_IMAGE}:${VERSION}-${VARIANT} ${DOCKER_IMAGE}:${VERSION}-${VARIANT}-${BUILD_NUMBER}"
        sh "docker push ${DOCKER_IMAGE}:${VERSION}-${VARIANT}-${BUILD_NUMBER}"
        sh "docker push ${DOCKER_IMAGE}:${VERSION}-${VARIANT}"
    }
}
