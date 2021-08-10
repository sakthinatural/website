pipeline {
    agent none
    
    stages {
        stage("Build Website"){
            agent {
                    label "testing"
                }
            steps {
                git 'https://github.com/sakthinatural/website.git'
                sh "sudo docker build . -t sakthinatural123/testing:latest"
                
            }
        }
        stage('Test Website') {
            agent {
                label "testing"
            }
            steps {
                sh "sudo docker run -it -d -P testing" 
            }
        }
        
        stage('Push to Docker Hub') {
            agent {
                label "testing"
            }
            steps {
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerpwd')]) {
                  sh "sudo docker login -u sakthinatural123 -p ${dockerpwd}"
                }
                sh "sudo docker push sakthinatural123/testing:latest"
                
               
            }
            
        }
        
        stage('Deploy to Kubernetes') {
            agent {
                label "testing"
            }
            steps {
                script {
                    kubernetesDeploy configs: 'deployment.yaml', kubeConfig: [path: ''], kubeconfigId: 'mykubeconfig', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
                }
            }
        }
        
    }
}

