pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script{
                    app=docker.build("cdanansuriya1992/sample-node")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'DockerHub') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        
        stage('DeployToProduction') {
            steps {
                input 'Deploy to Production?'
    
                script {
                    sh "docker pull cdanansuriya1992/sample-node:${env.BUILD_NUMBER}"
    
                    try {
                        sh "docker stop sample-node-app"
                        sh "docker rm sample-node-app"
                    } catch (err) {
                        echo: 'caught error: $err'
                    }
    
                    sh "docker run --name sample-node-app -d -p 80:3000 cdanansuriya1992/sample-node:${env.BUILD_NUMBER}"
    
                }
            }
        }   
    }
}
