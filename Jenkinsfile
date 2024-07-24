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
                withGradle {
                   bat './gradlew clean build --stacktrace -i'
                }
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

    }
}