pipeline{
    agent any
    tools {
        maven 'maven381'
        jdk 'Java'
    }
    stages{
        stage("Pull"){
            steps{
                git 'https://github.com/rahulpatilgit/DevOpsDemo.git'
            }
        }
        stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('Sonarqube') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
        stage("Deploy"){
            steps{
                sshagent(['tomcat']) {
                    sh "scp -o StrictHostKeyChecking=no /var/jenkins/workspace/Ass-3-Jenkinsfile/target/DevOpsDemo.war ec2-user@172.31.12.203:/tomcat/tomcat/webapps"
                }
            }
        }
        stage("Notify Status") {
            steps{
                slackSend channel: 'Jenkins-updates', message: "Build Status - ${env.JOB_NAME} ${env.BUILD_STATUS} (<${env.BUILD_URL}|Open>)"
            }
        }
    }
}
