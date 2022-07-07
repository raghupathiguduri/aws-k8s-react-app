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
            environment {
                dockerhub = "${params.dockercreds}"
                dockerusername = "${dockerhub_USR}"
                dockerpassword = "${dockerhub_PSW}"
                versionNumber = "${BUILD_NUMBER}"
            }
            steps {
                DockerBuild(versionNumber,dockerusername,dockerpassword)
            }
        }
    }
}
