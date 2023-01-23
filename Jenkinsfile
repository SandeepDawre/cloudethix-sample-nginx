pipeline {
    agent any 
    environment {
        registryURI = "https://registry.hub.docker.com/"
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
            environment {
                registry_endpoint = "${env.registryURI}" + "${env.registry}"
            }
            steps{
                script {
                    docker.withRegistry( registry_endpoint, registryCredential ) {
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
