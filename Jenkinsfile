pipeline {
    agent any //bisa juga di kasih di stage
    environment {
        AUTHOR = "Ichwan Dwi Nursid"
        EMAIL = "vV6xq@example.com"
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 15, unit: 'MINUTES')
    }
    parameters {
        string(name: "NAME", defaultValue:"Ichwan", description: "What is your name?")
        text(name: "DESCRIPTION", defaultValue:"", description: "Tell Me About You?")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to Deploy")
        choice(name: "SOCIAL_MEDIA", choices: ["Instagram", "Facebook", "Twitter"], description: "Pick something")
        password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
    }

    // triggers {
    //     cron('*/5 * * * *')
    //     pollSCM('*/5 * * * *')
    //     upstream(upstreamProjects: 'job1, job2', threshold: hudson.model.Result.SUCCESS)
    // }
    stages {
        stage("OS_SETUP"){ //Matrix
            matrix{
                axes {
                    axis {
                        name "OS"
                        values "Windows", "Linux", "MacOS"
                    }
                    axis {
                        name "ARCH"
                        values "32", "64"
                    }
                }

                stages{
                    stage("OS SETUP"){
                        steps {
                            echo "Hello ${OS} ${ARCH}"
                        }
                    }
                }
            }
           
        }
        stage("preparation"){
            parallel{
                stage("nodejs preparation"){
                    steps {
                        echo "Hello NodeJs"
                    }
                }
                stage("npm preparation"){
                    steps {
                        echo "Hello Npm"
                    }
                }
            }
        }
        stage("prepare"){
            steps {
                echo "start jonb : ${env.JOB_NAME}"
                echo "start build : ${env.BUILD_NUMBER}"
                echo "Branch Name : ${env.BRANCH_NAME}"
                script {
                    def buildDisplay = currentBuild.getDisplayName()
                    echo "build display : ${buildDisplay}"
                }
            }
        }
        stage("Build"){
            environment {
                APP = credentials("my-password") // get from jenkins credentials 
            }
            steps {
                echo "username = ${APP_USR}" // username di beri suffx USR akan di reject
                echo "password = ${APP_PSW}" // password di beri suffx PSW akan di reject
                echo "Hello Build"
                sleep(5)
                echo "Hello Build"
                script {
                    for (int i = 0; i < 10; i++) {
                        echo "Hello ${i}"
                    }
                }
                sh 'echo "app password = $APP_PSW" > "rahasia.txt"'
            }
        }

        stage("Test"){
            steps {
                echo "Hello Test"
                sleep(5)
                echo "Hello Test"
                script {
                    def data = [    // object pakeknya array
                        "firstName" : "Ichwan",
                        "lastName" : "Nursid"
                    ]

                    writeJSON(file: "data.json", json: data)
                }
            }
        }

        stage("Deploy"){
            input {
                message "Can we Deploy?"
                ok "Yes, Of course"
                submitter "ichwan,nursid"
                parameters {
                    choice(name: "TARGET_ENV" , choices: ["DEV", "PROD","QA"], description:" Which Environment?")
                }
            }
            steps {
                // echo "AUTHOR = ${AUTHOR}"
                // echo "EMAIL = ${EMAIL}"
                echo "parameter TARGET_ENV = ${params.TARGET_ENV}"
                sleep(5)
                echo "Hello Deploy"
                sleep(10)
                echo "Hello Deploy"
                sh "ls -la"
            }
        }

        stage("Parameter"){
            steps {
                echo "NAME = ${params.NAME}"
                echo "DESCRIPTION = ${params.DESCRIPTION}"
                echo "DEPLOY = ${params.DEPLOY}"
                sleep(10)
                echo "SECRET = ${params.SECRET}"
            }
        }

        stage("release"){
            when {
                expression {
                    params.DEPLOY == true
                }
            }
            steps {
                echo "Hello Release"
                sleep(5)
                echo "Hello Release"
            }
        }
    }
    post {
       always {
        echo "always"
       }
       success {
        echo "success"
       }
       failure {
        echo "failure"
       }
    }
}