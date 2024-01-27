pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('DOCKER_HUB')

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
                    docker build -t muthuarumugam/testapp:${BUILD_NUMBER} .
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
                    docker push muthuarumugam/testapp:${BUILD_NUMBER}
                    '''
                
                }
            }
        }


        stage('deploy'){
            steps{
                script{
                    sh '''
                    echo 'Deploy'
                    docker pull muthuarumugam/testapp:${BUILD_NUMBER}
                    docker run -d --name testapp -p 5000:5000 muthuarumugam/testapp:${BUILD_NUMBER}
                    '''
                
                }
            }
        }
        
 
    }  
}
