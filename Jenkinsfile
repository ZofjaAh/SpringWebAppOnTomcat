#!/usr/bin/env groovy

pipeline {
    agent any
    triggers {
         pollSCM('* * * * *')
     }
     environment {
                     TOMCAT_CREDS=credentials('TomcatCreds')
                     TOMCAT_SERVER="https://8d04-54-36-174-39.ngrok-free.app/"
                     ROOT_WAR_LOCATION="/opt/tomcat/webapps"
                     LOCAL_WAR_DIR="build/dist"
                     WAR_FILE="simple-web-app.war"
                   }
    stages {

          stage('Build') {
              steps {
                withGradle {
                   bat './gradlew clean build --stacktrace -i'
                }
              }
            }


            }
        }