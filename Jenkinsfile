pipeline {
    agent any

    tools {
        jdk "jdk17"
        maven "M3"
    }
    environment {
        AWS_CREDENTIAL_NAME = "AWSCredentials"
        REGION = "ap-northeast-2"
        DOCKER_IMAGE_NAME="aws02-spring-petclinic"
        ECR_REPOSITORY = "257307634175.dkr.ecr.ap-northeast-2.amazonaws.com"
        ECR_DOCKER_IMAGE = "${ECR_REPOSITORY}/${DOCKER_IMAGE_NAME}"
    }
    
    stages {
        stage('Git Clone') {
            steps {
                echo 'Git Clone'
                git url: 'https://github.com/Swiepr/spring-petclinic.git',
                branch: 'wavefront'
            }
            post {
                success {
                    echo 'success clone project'
                }
                failure {
                    error 'fail clone project' // exit pipeline
                }
            }
        }        
        stage ('mvn Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml' 
                }
            }
        }
    }
}
