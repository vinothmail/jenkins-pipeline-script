pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        app_name  = "Reaper"
        user_name = "Vinoth Padmanaban"
        location  = "Chennai"
        email     = "vinoth.mail@gmail.com"
        branch    = "master"
    }

    stages {
        stage('Build') {
            steps {
                echo 'building'
                echo "${app_name}"
            }
            post {
                success {
                    echo 'build completed.'
                    bat 'date /t'
                }
                failure {
                    echo 'build incomplete'
                }
            }
        }

        stage('Test') {
            when {
                expression { env.GIT_BRANCH in ['origin/main', 'origin/master', 'main', 'master'] }
            }
            steps {
                echo 'Testing has started'
                echo "${branch}"
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
