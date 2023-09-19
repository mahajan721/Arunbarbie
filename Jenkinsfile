pipeline {
agent any
stages {
stage('Build') {
steps {
sh 'mvn clean package'
}
}
stage('Unit and Integration Tests') {
steps{
sh 'mvn test'
}
}
stage('Code Analysis') {
steps{
withSonarQubeEn('SonarQube'){
sh 'mvn sonar:sonar'
}
}
}
stage('Security Scan') {
steps{
sh 'zap-baseline.py -t http://localhost:8080 -r security-report.html'

archiveArtifacts 'security-report.html'
}
}
stage('Deploy to Staging') {
steps{
sshagent(['my-ssh-credentials']) 
{
sh 'ssh user@stagging-server "cd /path/to/application && ./deploy.sh" '
}
}
}
stage('Integration Tests on Staging'){
steps{
sh 'mvn test'
}
}
stage('Deploy to Production'){
steps{
sshagent([ 'my-ssh-credentials']) 
{
sh 'ssh user@production-server "cd /path/to/application && ./deploy.sh" '
}
}
}
}
post {
always {
mail to: "barbiemahajan721@gmail.com",
subject:"Pipeline status",
body: "Build log attached!"
}
}
}

