pipeline {
    agent any
    
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
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerpwd')]) {
                    sh 'sudo docker login -u sakthinatural123 -p ${dockerpwd}'
                    sh "sudo docker run -it -d -P sakthinatural123/testing:latest" 
                 }  
               
            }
        }
        
        stage('Push to Docker Hub') {
            agent {
                label "testing"
            }
            steps {
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerpwd')]) {
                    sh 'sudo docker login -u sakthinatural123 -p ${dockerpwd}'
                    sh "sudo docker push sakthinatural123/testing:latest"
                 }  
                
            }
            
        }
        
        
        stage('Copy file to  k8s master') {
          agent {
                label "testing"
          }
          steps {
               sshagent(['sshkey']) {
                  sh "scp -o StrictHostKeyChecking=no deployment.yaml ubuntu@172.31.26.16:/home/ubuntu"
              }
          }
      }

      stage('Deploy App on k8s') {
          steps {
               sshagent(['sshkey']) {
                sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.26.16 -C \"sudo kubectl apply -f deployment.yaml\""
              }
          }
      }
                
        
    }
}
