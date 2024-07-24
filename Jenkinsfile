#!/usr/bin/env groovy

pipeline {

    agent any

    triggers {
         cron('H */8 * * *')
         pollSCM('* * * * *')
     }
    stages {
        stage('Build') {
            steps {
                 sh './gradlew clean build'
            }
        }
        stage('deployment') {
            steps {
                deploy adapters: [tomcat10(url: 'https://8d04-54-36-174-39.ngrok-free.app/',
                credentialsID: 'TomcatCreds')],
                war: 'target/*.war',
                contextPath: 'app'
            }
        }
        stage('Notification')
        steps{
        emailext(
        subject: "Job Completed",
        body: "Jenkins pipeline job for maven build job completed",
        to: "zozolulu1894@gmail.com"
        )
        }
    }
}