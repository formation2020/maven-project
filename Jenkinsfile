/* groovylint-disable CompileStatic */
pipeline {
    agent any
    /* groovylint-disable-next-line SpaceBeforeOpeningBrace */
    stages{
        stage('Build')
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                build job : 'Deploy-To-Staging'
            }
        }
        stage('Deploy to Prod') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message : 'approve production deployment'
                }
                build job : 'deploy-to-Production'
            }
            post {
                success {
                    echo 'Code deployed to production '
                }
                failure {
                    echo 'deployement  to production failed  '
                }
            }
        }
    }
