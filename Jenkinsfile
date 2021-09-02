pipeline{
    agent any

    stages{
        stage('Get Source'){
            steps{
                git url: 'https://github.com/cristianosaito/nodeapp.git', branch: 'master'
            }
        }

        stage('Docker Build'){
            steps{
                script{
                    dockerapp = docker.build("cristianosaito/nodeapp:v${env.BUILD_ID}", '-f ./Dockerfile .')
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

        stage('Deploy K8s'){
            agent {
                kubernetes{
                    cloud 'kubernetes'
                }
            }

            steps{
                kubernetesDeploy(configs: '**/k8s/**' kubeconfigId: 'kubeconfig')
            }
        }

        // stage('Deploy k8s') {
        //     agent { 
        //         kubernetes { 
        //             cloud 'kubernetes'
        //         }
        //     }
        //     environment {
        //         tag_version = "${env.BUILD_ID}"
        //     }
        //     steps {
        //         script {
        //             sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/api/deployment.yaml'              
        //             sh 'cat ./k8s/api/deployment.yaml'
        //             kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig')
        //             kubernetesDeploy(configs: '**/nginx/**', kubeconfigId: 'kubeconfig')
        //         }             
        //     }
        // }
    }
}