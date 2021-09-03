node {


def TENANT='807152304871829504'    // Please modify with your tenand id value
def CLOUD='cto-demo-rbc'          // Please modify with your cloud account value
def GROUP='demo-build'           //Please modify with your cluster name where you deployed the enforcer
def APOCTL_API="https://api.east-02.network.prismacloud.io" // Please modify with Console API


stage('Clone repository') {
checkout scm
}

stage('Download apoctl') {
sh "curl -o /usr/local/bin/apoctl https://download.aporeto.com/apoctl/linux/apoctl"
sh "sudo chmod +x /usr/local/bin/apoctl"
}

stage('Check if the authorization works') {
// Before this step you need to create the credencials file in the console at /TENANT/CLOUD namespace and import this file as a secret in Jenkins

withCredentials([file(credentialsId: 'pipeline.creds', variable: 'APOCTL_CREDS')]){
 sh "apoctl auth verify"
}

}
stage('Deploy app and Policy as Code') {
withCredentials([file(credentialsId: 'pipeline.creds', variable: 'APOCTL_CREDS')]) {
withEnv (["TENANT=${TENANT}","CLOUD=${CLOUD}","GROUP=${GROUP}", "APOCTL_API=${APOCTL_API}"]) {

 // Deploying app and policies
sh "all/create"

}
}

}
}
