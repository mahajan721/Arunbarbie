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
  post{
    success{
      mail to: 'barbiemahajan721@gmail.com',
        subject: 'Unit and Integration Tests Passed',
        body: 'The unit and Integration tests have passed. See aatached logs for more information.',
        attachLog:true 
    }
    failure{
      mail to: 'barbiemahajan721@gmail.com,'
      subject:'Unit and Integration Tests Failed',
        body: 'The Unit and Integration tests failed. See attached logs for more information.',
        attachLog:true
    }
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
  post {
    success{
      mail to: 'barbiemahajan721@gmail.com',
        subject: 'Security Scan',
        body: 'The security scan has passed.See attached logs for more information.',
        attachLog: true
    }
    failure{
      mail to: 'barbiemahajan721@gmail.com',
        subject: 'Security Scan',
        body: 'The security scan has failed. See attached logs for more information.',
        attachLog: true
    }
      
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



