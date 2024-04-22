pipeline {
    agent any 
   
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/pgvda/3829-AriyarathnaPGRVD/tree/main/Ariyarathna_PGRVD'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t adomicarts/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-docker', variable: 'dockerpw')]) {
   
               bat'docker login -u adomicarts -p ${dockerpw}'
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push adomicarts/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}