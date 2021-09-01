pipeline{
    agent any

    stages{
        stage('Get Source'){
            steps{
                git url: 'https://github.com/cristianosaito/nodeapp.git', branch: 'Master'
            }
        }

        stage('Docker Build'){
            steps{
                script{
                    dockerapp = docker.build("cristianosaito/nodeapp:${env.BUILD_ID}", '-f ./Dockerfile .')
                }
            }
        }

        stage('Docker Push Image'){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}