node {
    environment {
        VERSION = '10-2.4'
        VARIANT = 'alpine'
        DOCKER_IMAGE = 'dvitali/postgis'
    }
    stage('Checkout') {
        checkout scm
    }

    stage('Build Image'){
        sh "cd ${env.VERSION}/${env.VARIANT}/"
        sh "docker build --pull --no-cache -t ${env.DOCKER_IMAGE}:${env.VERSION}-${env.VARIANT} ."
    }

    stage('Push Image') {
        withCredentials([usernamePassword(
            credentialsId: "docker-hub-dvitali",
            usernameVariable: "USER",
            passwordVariable: "PASS"
        )]) {
            sh "docker login -u $USER -p $PASS"
        }

        sh "docker tag ${env.DOCKER_IMAGE}:${env.VERSION}-${env.VARIANT} ${env.DOCKER_IMAGE}:${env.VERSION}-${env.VARIANT}-${env.BUILD_NUMBER}"
        sh "docker push ${env.DOCKER_IMAGE}:${env.VERSION}-${env.VARIANT}-${env.BUILD_NUMBER}"
        sh "docker push ${env.DOCKER_IMAGE}:${env.VERSION}-${env.VARIANT}"
    }
}
