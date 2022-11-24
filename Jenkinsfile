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
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
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
    }
}
