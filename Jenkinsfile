#!/usr/bin/env groovy

pipeline {
    agent any
    triggers {
         cron('H */8 * * *')
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
            stage('copy the war file to the Tomcat server') {
               steps {
                    bat '''
                      ssh -i $TOMCAT_CREDS $TOMCAT_CREDS_USR@$TOMCAT_SERVER "/home/pi/tools/apache-tomcat-10.1.18/bin/catalina.sh stop"
                      ssh -i $TOMCAT_CREDS $TOMCAT_CREDS_USR@$TOMCAT_SERVER "rm -rf $ROOT_WAR_LOCATION/ROOT; rm -f $ROOT_WAR_LOCATION/ROOT.war"
                      scp -i $TOMCAT_CREDS $LOCAL_WAR_DIR/$WAR_FILE $TOMCAT_CREDS_USR@$TOMCAT_SERVER:$ROOT_WAR_LOCATION/ROOT.war
                      ssh -i $TOMCAT_CREDS $TOMCAT_CREDS_USR@$TOMCAT_SERVER "chown $TOMCAT_CREDS_USR:$TOMCAT_CREDS_USR $ROOT_WAR_LOCATION/ROOT.war"
                      ssh -i $TOMCAT_CREDS $TOMCAT_CREDS_USR@$TOMCAT_SERVER "/home/pi/tools/apache-tomcat-10.1.18/bin/catalina.sh start"
                    '''
                    }
               }

            }
        }