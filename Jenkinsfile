@Library("jenkins-shared-library@kaiburr") _
pipeline {
    agent any
    tools {
        nodejs 'nodejs-16.12'
    }
    stages {
        stage('Checkout') {
            steps {
              cleanWs()
              echo "Checking out....."
              checkout scm
            }
        }
        /*stage('Init Version') {
            when {
                expression { params.BRANCH == 'develop' }
            }         
            steps {
                BumpVersion()
            }
        }*/
        stage('Build App') {
            when {
                expression { params.BRANCH != 'develop' }
            }         
            steps {
                NPMBuild()
            }
        }
        
        stage('Create Prod Build') {
            when {
                expression { params.BRANCH == 'develop' }
            }         
            steps {
                NPMBuild("prod")
            }
        }
        stage('Docker Build') {
            when {
                expression { params.BRANCH == 'develop' }
            }
            steps {
                dockerhub = "${params.dockercreds}"
                versionNumber = ${BUILD_NUMBER}
                DockerBuild(versionNumber,dockerhub_USR,dockerhub_PSW)
            }
        }
    }
}
