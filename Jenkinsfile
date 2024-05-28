#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/lpadgett23/jenkins-shared-library.git',
    credentialsID: 'gitlab-credentials'
    ]
)

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    environment {
        IMAGE_NAME = 'lepcloud23/demo-app:java-maven-app-aws-5.1'
    }
    stages {
        stage('build app') {
            steps {
                echo 'building application jar...'
                buildJar()
            }
        }
        stage('build image') {
            steps {
                script {
                    echo 'building the docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        } 
        stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to EC2 with docker-compose up...'
                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                    sshagent(['ec2-server-ssh-key']) {
                        sh "scp docker-compose.yaml ec2-user@18.232.67.153:/home/ec2-user"  // jenkins is holding the checked out repo directory right now
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.232.67.153 ${dockerComposeCmd}"
                    }
                }
            }               
        }
    }
}
