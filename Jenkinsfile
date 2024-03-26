pipeline {
    agent any
    tools{
        maven 'M3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/babu265/spingboot-demo']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t rameshaws22/spring-boot-demo .'
                }
            }
        }
                stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u rameshaws22 -p ${dockerhubpwd}'}
                   sh 'docker push rameshaws22/spring-boot-demo'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                     kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8skubeconfig')
                 }
            }
        }
        
    }
}
