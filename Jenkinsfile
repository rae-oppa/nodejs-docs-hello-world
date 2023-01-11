/* Nodejs Web Server Docker Based Jenkinsfile */
pipeline {
    agent any

    options {
        timeout(time: 2, unit: 'HOURS')
    }

    environment {
        REPO_URL = sh(returnStdout: true, script: 'git config --get remote.origin.url').trim()
        REPO_NAME = sh(returnStdout: true, script: 'git config --get remote.origin.url | rev | cut -f 1 -d "/" | rev | sed "s/.git//gi";sed "/^ *$/d"').toLowerCase().trim() 
        //PORT = sh(returnStdout: true, script: 'cat docker/Dockerfile | egrep EXPOSE | awk \'{print $2}\'').trim()
        //APP = ' '
    }

    stages {
        /*stage('Clone') {             
            steps {
                echo 'Clone'
                git branch: 'main', credentialsId: 'dsshin', url: 'https://github.com/dsshin-virnect/nodejs-docs-hello-world.git'
            }
         }*/
        
        stage ('build docker image') {
            steps {
                script {
                    sh '''
                    docker build --tag helloworld:$BUILD_NUMBER
                    docker stop helloworld && docker rm helloworld
                    docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
                    '''
                }
            }
        }     
    }
}
