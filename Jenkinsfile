pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Initiliaze Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-credentials', url: 'https://github.com/TahaAydas/devops-automation']]])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t tahaaydas/devops-integration .'
                }
            }
        }
        stage('Push image to DockerHub'){
            steps{
                script{
                   
                   bat 'docker login -u tahaaydas -p Tersane13'


                   
                   bat 'docker push tahaaydas/devops-integration:latest'
                }
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                script{
                    kubernetesDeploy configs: 'deploymentservice.yaml', kubeConfig: [path: ''], kubeconfigId: 'k8configpwd', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']                    
                }
            }
        }
    }
}