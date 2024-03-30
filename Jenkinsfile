pipeline {
    agent any

    environment {
        registry = "257307634175.dkr.ecr.ap-northeast-2.amazonaws.com/project01-test4-ecr"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Swiepr/spring-petclinic.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-northeast-2 | docker login --username swiep --password-stdin 257307634175.dkr.ecr.ap-northeast-2.amazonaws.com/project01-test4-ecr"
                    sh "docker push 257307634175.dkr.ecr.ap-northeast-2.amazonaws.com/project01-test4-ecr"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-21 springboot-0.1.0.tgz"
                }
            }
    }
}
