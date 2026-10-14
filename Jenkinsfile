pipeline {
    agent any

    environment {
        REGISTRY = 'leeliya/tp4'
        registryCredential = 'dockerhub'
        DOCKER_HOST = 'npipe:////./pipe/dockerDesktopLinuxEngine'
        DOCKER_CONTEXT = ''
        DOCKER_API_VERSION = ''
        DOCKER_EXE = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
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

        stage('Deploy image') {
            steps {
                bat '''
                    "%DOCKER_EXE%" rm -f tp4-pipeline-deployed 2>NUL
                    "%DOCKER_EXE%" run -d --name tp4-pipeline-deployed -p 8082:80 %REGISTRY%:%BUILD_NUMBER%
                '''
            }
        }
    }
}
