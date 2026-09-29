pipeline {
    agent any

    tools {
        nodejs 'node20'
    }

    parameters {
        string(
            name: 'APP_PORT',
            defaultValue: '3000',
            description: 'Server Port'
        )
    }

    environment {
        IMAGE_NAME = 'jenkins-demo-app'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from git repo'
            }
        }
        stage('Check Docker') {
            steps {
                bat 'docker --version'
            }
        }
        stage('Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Test App') {
            steps {
                bat 'npm test'
            }
        }
        stage('Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }
        stage('Run Container') {
            steps {
                bat '''
                    for /f "tokens=*" %%i in ('docker ps -q --filter "publish=%APP_PORT%"') do docker rm -f %%i
                    docker run -d --name node-app-%BUILD_NUMBER% -p %APP_PORT%:3000 %IMAGE_NAME%:%BUILD_NUMBER%
                '''
            }
        }
        stage('Verify') {
            steps {
                bat '''
                    echo App deployed successfully
                    echo Open
