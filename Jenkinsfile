/* Nodejs Web Server Docker Based Jenkinsfile */
pipeline {
    agent any

    options {
        timeout(time: 2, unit: 'HOURS')
    }

    environment {
        REPO_URL = sh(returnStdout: true, script: 'git config --get remote.origin.url').trim()
        REPO_NAME = sh(returnStdout: true, script: 'git config --get remote.origin.url | rev | cut -f 1 -d "/" | rev | sed "s/.git//gi";sed "/^ *$/d"').toLowerCase().trim() 
        PORT = sh(returnStdout: true, script: 'cat docker/Dockerfile | egrep EXPOSE | awk \'{print $2}\'').trim()
        BRANCH_NAME = "${BRANCH_NAME.toLowerCase().trim()}"
        //APP = ' '
    }

    stages {
        /*stage ('edit package version') {
            steps {
                script {
                    if ("${BRANCH_NAME}" == 'main') {
                      sh '''
                        sed -i "/\\"version\\":/ c\\  \\"version\\": \\"${NEXT_VERSION}\\"," package.json
                      '''
                    } else {
                      sh '''
                        sed -i "/\\"version\\":/ c\\  \\"version\\": \\"${NEXT_VERSION}-${BRANCH_NAME}-${BUILD_NUMBER}\\"," package.json
                      '''
                    }
                }
            }
        }*/
        stage('Clone') {             
            steps {
                echo 'Clone'
                git branch: 'main', credentialsId: 'dsshin', url: 'https://github.com/dsshin-virnect/nodejs-docs-hello-world.git'
            }
         }
        
        stage ('build docker image') {
            steps {
                script {
                    //APP = docker.build("""${REPO_NAME}:${BRANCH_NAME}-${BUILD_NUMBER}""", """--build-arg -f ./docker/Dockerfile .""")
                    docker build --tag helloworld:$BUILD_NUMBER
                    //docker stop helloworld && docker rm helloworld
                    //docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
                }
            }
        }     

        /*stage ('save image to acr') {
            when { anyOf { branch 'main'; branch 'master' } }
            steps {
                script {
                    docker.withRegistry("https://$aws_ecr_address", 'ecr:ap-northeast-2:aws-ecr-credentials') {
                        APP.push("${NEXT_VERSION}-${BRANCH_NAME}-${BUILD_NUMBER}")

                        if ("${BRANCH_NAME}" == 'main') {
                            APP.push("${NEXT_VERSION}")
                            APP.push("latest")
                        }
                    }
                }
            }
        }*/
    }
}
