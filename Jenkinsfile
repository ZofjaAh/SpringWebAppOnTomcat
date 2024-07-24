#!/usr/bin/env groovy

pipeline {
    agent any
    triggers {
         cron('H */8 * * *')
         pollSCM('* * * * *')
     }
    stages {
     stage('Clone sources') {
            git url: 'https://github.com/ZofjaAh/SpringWebAppOnTomcat.git'
        }
    stage('Artifactory configuration') {
            rtGradle.tool = "Gradle 8.3"
            rtGradle.deployer repo:'ext-release-local', server: server
            rtGradle.resolver repo:'remote-repos', server: server
        }
        stage('Build') {
            buildInfo = rtGradle.run
            rootDir: "gradle-examples/4/gradle-example-ci-server/", buildFile: 'build.gradle', tasks: 'clean artifactoryPublish'
              }
        }
        stage('Publish build info') {
                server.publishBuildInfo buildInfo
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