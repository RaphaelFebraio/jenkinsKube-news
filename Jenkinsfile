pipeline {
   agent any

   stages {
    
        stage ('Builder Docker Image') {
            steps {
                script{
                    dockcerapp = docker.build("raphaelfebraio/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub'){
                    dockcerapp.push('latest')
                    dockcerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy Kubernets') {

            steps {
                withKubeconfig (credentialsId: 'kubeconfig') {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                } 
            }

        }

   }

}