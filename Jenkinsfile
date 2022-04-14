#!/usr/bin/env groovy
// shebang tells most editors to treat as groovy (syntax highlights, formatting, etc)

pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        // Implicit when in repo
        // stage('Checkout') {
        //     steps {
        //         git url: 'https://github.com/MikeAlMartinez/jgsu-spring-petclinic.git', branch: 'main'
        //     }
        // }
        stage('Clean') {
            steps {
                sh './mvnw clean'
            }
        }
        stage('Compile') {
            steps {
                sh './mvnw compile'
            }
        }
        stage('Generate') {
            steps {
                sh './mvnw generate-resources'
            }
        }
        stage('Package') {
            steps {
                sh './mvnw package'
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
        }
        success {
            archiveArtifacts 'target/*.jar'
        // }
        // changed {
            emailext subject: "Job \'${JOB_NAME}\' (${BUILD_NUMBER}) ${currentBuild.result}",
                body: "Please go to ${BUILD_URL} and verify the build",
                attachLog: true,
                compressLog: true,
                recipientProviders: [buildUser(), requestor()],
                to: "test@jenkins"
        }
    }
}
