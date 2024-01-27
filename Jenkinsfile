pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('DOCKER_HUB')
        DOCKER_CONTAINER = "mycontainer"
        DOCKER_IMAGE =  "muthuarumugam/testapp"

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


        stage('deploy'){
            steps{
                script{
                    sh '''
                    echo 'Deploy'
                    docker pull ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    docker stop $DOCKER_CONTAINER || true && docker rm $DOCKER_CONTAINER || true
                    docker run -d --name $DOCKER_CONTAINER -p 5002:5000 muthuarumugam/testapp:${BUILD_NUMBER}
                    '''
                
                }
            }
        }
        
 
    }  
}
