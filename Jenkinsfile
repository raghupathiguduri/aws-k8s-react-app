@Library("jenkins-shared-library@master") _
pipeline {
    agent any
    tools {
        nodejs 'nodejs-16.12'
    }
    stages {
        stage('Checkout') {
            steps {
              deleteDir()
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
                DockerBuild()
            }
        }
    }
}
