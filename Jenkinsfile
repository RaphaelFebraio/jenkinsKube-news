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

   }

}