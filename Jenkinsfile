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
                echo "${env.app_name}"
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
                writeFile file: 'report.txt',
                    text: "Location: ${location}, ${email}"
                archiveArtifacts artifacts: 'report.txt'
                bat 'type report.txt'
                   }
        }
        stage('Write File') {
            steps {
                writeFile file: 'sample.txt', text: '''
Application: REAPER
Version: 1.7
Environment: UAT
'''
                script {
                    def content = readFile('sample.txt')
                    echo content
                }
            }
        }
        stage('Deploy') {
            options {
                timeout(time: 3, unit: 'MINUTES')
                        }
            input {
                message 'Deploy to production?'
                ok 'Yes, deploy'
                submitter 'Vinoth,admin'
                submitterParameter 'APPROVER'
                parameters {
                    string(name: 'REASON', defaultValue: '', description: 'why deploy?')
            }
            }
            steps {
                echo 'deploying...'
                echo "Authorized by: ${APPROVER}"
                writeFile file: 'deploy-audit.txt', text: """\
        App        : ${app_name}
        Build      : ${env.BUILD_NUMBER}
        Approver   : ${APPROVER}
        Reason     : ${REASON}
        Time       : ${new Date()}
        Build URL  : ${env.BUILD_URL}
        """
                archiveArtifacts artifacts: 'deploy-audit.txt', fingerprint: true
            }
            post {
                aborted {
                     echo 'Deploy was aborted or timed out.'
                        }
                    }
                }
       
}
