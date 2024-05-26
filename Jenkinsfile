#!/usr/bin.env groovy

pipeline {   
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application..."

                }
            }
        }
        stage("build") {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    sshagent(['ec2-server-ssh-key']) {
                        def dockerCmd = "docker run -p 3080:8080 -d lepcloud23/demo-app:4.1.2-10"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.232.67.153 ${dockerCmd}"
                    }
                }
            }
        }               
    }
} 
