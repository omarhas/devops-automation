pipeline {
    agent any
    tools {
        maven "MAVEN3"
        
    }
    stages{
        stage('build maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/omarhas/devops-automation']])
                sh "mvn clean install"
            }
        }
        
        stage('build dockerfile'){
            
            steps{
                script{
                    sh 'docker build -t omarhasni123/devops-integration .'
                }
            }
        }
        
        stage('push image to dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
    // some block
                    sh 'docker login -u omarhasni123 -p ${dockerhubpwd}'
                    sh 'docker push omarhasni123/devops-integration'
}
                }
            }
        }
                stage('Deploy from Docker Registry') {
            steps {
                script {
                    sh "docker pull omarhasni123/devops-integration:latest"
                    sh "docker run -d --name java-app -p 80:80 omarhasni123/devops-integration:latest"
                }
            }
        }

        
    }
    
}