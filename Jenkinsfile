def SONARQUBE_URL="http://lnxhom048.rootbrasil.intranet:9000"
def MY_PROJECT_KEY="CVP"
def REPO="https://dev.azure.com/Delivery-Un2/P20220937%20-%20CVP%20-%20Downsizing/_git/P20220937%20-%20CVP%20-%20Downsizing:/usr/src"

pipeline {
    agent any
        stages {
           stage('Sonar scanner') {
            steps {
              script {  
                def scannerHome = tool 'Sonarqube';   
                withSonarQubeEnv('Sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=CVP -Dsonar.sources=. -Dsonar.host.url=http://lnxhom048.rootbrasil.intranet:9000 -Dsonar.token=sqp_4a698719d36f300eaa8c46ac9546e5cf84424805"
              }
            }
           }
         }
          
        stage('Docker push and build') {
            steps {
                withDockerRegistry([credentialsId: "Docker hub", url: ""]) { 
                  sh 'printenv'
                  sh 'docker build -t thiagodockerid/cvp-app:latest .'
                  sh 'docker push thiagodockerid/cvp-app:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                withKubeConfig([credentialsId: 'kubernetes']) {
                sh "sed -i 's#replace#thiagodockerid/cvp-app:latest#g' k8s/deployment.yaml"
                sh 'kubectl apply -f k8s/deployment.yaml' 
                sh 'kubectl get pod'
  
            }
        }
    }
  }
}