pipeline {
    agent any 
    environment {
        registry = "teamcloudethix/cloudethix-sample-nginx"
        registryCredential = '02_docker_hub_creds'
        }
stages {
        stage('Building image from project dir') {
            steps{
                script {
                def app = docker.build registry + ":$GIT_COMMIT"
                }
            }
        }
        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( registry, registryCredential ) {
                        app.push()
                        app.push(latest)
                    }   
                }
            }   
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$GIT_COMMIT"
                sh "docker rmi $registry:latest"
                }
        }
    }
    post { 
        always { 
            echo 'Deleting Workspace'
            deleteDir() /* clean up our workspace */
        }
    }
}