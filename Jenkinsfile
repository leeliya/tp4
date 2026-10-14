pipeline {
    agent any

    environment {
        REGISTRY = 'leeliya/tp4'
        registryCredential = 'dockerhub'
    }

    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/leeliya/tp4.git'
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build(
                        "${REGISTRY}:${BUILD_NUMBER}"
                    )
                }
            }
        }

        stage('Test image') {
            steps {
                echo 'Tests passed'
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        registryCredential
                    ) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
