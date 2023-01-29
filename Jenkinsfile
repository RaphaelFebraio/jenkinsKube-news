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
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                withKubeConfig ([credentialsId: 'kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                } 
            }

        }

   }

}