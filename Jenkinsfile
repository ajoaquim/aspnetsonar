COREHOST_TRACE=1def SONARQUBE_URL="http://lnxhom048.rootbrasil.intranet:9000"
def MY_PROJECT_KEY="CVP"
def REPO="https://dev.azure.com/Delivery-Un2/P20220937%20-%20CVP%20-%20Downsizing/_git/P20220937%20-%20CVP%20-%20Downsizing:/usr/src"

pipeline {
    agent any
        stages {
           stage('Sonar scanner') {
            steps {
              script {  
                scannerHome = tool 'Sonarqube-msbuild'
                withSonarQubeEnv() {
                    sh 'export COREHOST_TRACE=1'
                    sh 'dotnet ${scannerHome}/Sonarscanner begin /k:"aspnetsonar" /d:sonar.host.url="http://lnxhom048.rootbrasil.intranet:9000"  /d:sonar.token="sqp_bdb5d3987eb7a9478f4aeadf8ac0f895cc138a6a"'
                    sh 'dotnet build' 
                    sh 'dotnet ${scannerHome}/Sonarscanner end /d:sonar.token="sqp_bdb5d3987eb7a9478f4aeadf8ac0f895cc138a6a"'            
              }
            }
           }
         }
          
        /*stage('Docker push and build') {
            steps {
                withDockerRegistry([credentialsId: "Docker hub", url: ""]) { 
                  sh 'printenv'
                  sh 'docker build -t thiagodockerid/cvp-app:latest .'
                  sh 'docker push thiagodockerid/cvp-app:latest'
                }
            }
        }*/
        /*stage('Deploy') {
            steps {
                withKubeConfig([credentialsId: 'kubernetes']) {
                sh "sed -i 's#replace#thiagodockerid/cvp-app:latest#g' k8s/deployment.yaml"
                sh 'kubectl apply -f k8s/deployment.yaml' 
                sh 'kubectl get pod'
  
            }
        }
    }*/
  }
}
