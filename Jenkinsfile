pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('DOCKER_HUB')
        DOCKER_CONTAINER = "mycontainer"
        DOCKER_IMAGE =  "muthuarumugam/testapp2"

    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: 'JENKINS_GITHUB_SECRET', 
                url: 'https://github.com/Cloudlead20/jenkinsdocker',
                branch: 'master'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                    '''
                }
            }
        }



             stage('Login into Docker hub'){
                 steps{
                     script{
                         sh '''
                         echo 'Login into docker'
                         echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                         '''
                }
            }
        }

        stage('Push the dockerhub'){
            steps{
                script{
                    sh '''
                    echo 'Push'
                    docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    '''
                
                }
            }
        }


       
        
 
    }  
}
