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
        stage("Deploy"){
            steps{
                sshagent(['tomcat']) {
                    sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Ass-2-Nexus-Freestyle/target/DevOpsDemo.war ec2-user@172.31.12.203:/tomcat/tomcat/webapps"

                }
            }
        }
    }
}
