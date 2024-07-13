#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/ibrahim-osama-amin/Jenkins-shared-library.git',
     credentialsId: 'github-credentials'
    ]
)

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        IMAGE_NAME = 'ibrahimosama/my-repo:java-maven-2.0'
    }
    stages {
        stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   echo 'Logging into the docker repo'
                   withCredentials([usernamePassword(credentialsId:'docker-hub-repo',passwordVariable: 'PASS',usernameVariable: 'USER')]){
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push env.IMAGE_NAME"
                   }
                   
                }
            }
        }
    }
}