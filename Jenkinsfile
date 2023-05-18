pipeline{
    agent any
    
    environment{
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-snana')
    }
    
    
    stages {
        stage('Git clone from github'){
            steps{
                sh 'echo Git cloning begin'
                git 'https://github.com/isaacsnipe/nodejs-demo.git'
                sh 'echo Git clone completed'
            }
        }
        
        stage('Build Docker Image'){
            agent{ label "docker-agent" }
            steps{
                sh 'docker build -t snana/nodeapp:$BUILD_NUMBER .'
            }
        }
        
        stage('Login to Dockerhub'){
            agent { label "docker-agent" }
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Docker Deploy/Push image'){
            agent { label "docker-agent" }
            steps{
                sh 'docker push snana/nodeapp:$BUILD_NUMBER'
            }
        }
        
        stage('Logout Dockerhub'){
            agent { label "docker-agent "}
            steps{
                sh 'docker logout'
            }
        }
    }
}
