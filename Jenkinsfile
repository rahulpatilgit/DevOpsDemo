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
        stage("Build"){
            steps{
                sh 'mvn clean package'
               }
            }
        stage('Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "/opt/sonar/"
                }
                timeout(time: 10, unit: 'MINUTES') {
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
