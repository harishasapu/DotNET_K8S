 pipeline{
     agent any
     stages{
         stage('GitCheckout'){
             steps{
                 git branch: 'main', changelog: false, poll: false, url: 'https://github.com/harishasapu/DotNET_K8S.git'
             }
         }
          stage("Docker Build & tag"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                       sh "make image"
                    }
                }
            }
        }
        stage("Docker Push & Build"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker'){
                         sh "make push"
                         sh "docker run -d --name dotnet -p 5000:5000 harishasapu/dotnet:latest"
                    }
                }
            }
        }
         stage('Deploy to k8s'){
             steps{
                 dir('K8S'){
                     withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8-cred', namespace: '', restrictKubeConfigAccess: false, serverUrl: ''){
                         sh 'kubectl apply -f deployment.yaml'
                     }
                 }
             }
         }
     }
 }
