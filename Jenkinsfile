pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }
     parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch name')
    }

    environment {
        app_name  = "Reaper"
        user_name = "Vinoth Padmanaban"
        location  = "Chennai"
        email     = "vinoth.mail@gmail.com"
    }

    stages {
        stage('Build') {
            steps {
                echo 'building'
                echo "${app_name}"
                powershell 'Get-Date'
            }
            post {
                success {
                    echo 'build completed.'
                    echo "Authorized by: ${user_name}"
                    bat 'date /t'
                }
                failure {
                    echo 'build incomplete'
                }
            }
        }

        stage('Test') {
           when {
                expression { params.BRANCH == 'main' || params.BRANCH == 'master' }
            }
            steps {
                echo "testing branch ${params.BRANCH}"
            }            
            post {
                success {
                    echo 'testing completed.'
                }
            }
        }

        stage('Report') {
            steps {
                echo 'reporting...'
                echo "${location}, ${email}"
            }
        }

        stage('Deploy') {
            options {
                timeout(time: 3, unit: 'MINUTES')
                        }
            input {
                message 'Deploy to production?'
                ok 'Yes, deploy'
            }
            steps {
                echo 'deploying...'
            }
        }
    }
}
