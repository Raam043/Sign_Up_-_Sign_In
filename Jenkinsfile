node {
  def remote = [:]
  remote.name = 'Master'
  remote.host = '18.188.57.198'
  remote.user = 'root'
  remote.password = 'ramesh123'
  remote.allowAnyHosts = true
  stage('SSH-Kube Cleanup - Old project records') {
    sshCommand remote: remote, command: "kubectl delete all --all"
  }
}
pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Raam043/Sign_Up_-_Sign_In.git'
            }
        }
        stage('Docker Image Build') {
            steps {
                sh 'docker image rm -f raam043/login-project & docker build -t raam043/login-project .'
            }
        }
        stage('Docker Image Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'DP', variable: 'DP')]) {
                    sh 'docker login -u raam043 -p ${DP}'
                    sh 'docker push raam043/login-project'
            }
        }
        }
        stage('Deploying Application on Kubernetes') {
            steps {
                kubernetesDeploy configs: 'deployment.yml', kubeConfig: [path: ''], kubeconfigId: 'Kube-master-cnfg', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
            }
        }
        
    }    
}
