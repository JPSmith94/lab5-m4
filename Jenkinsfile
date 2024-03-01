pipeline {
    agent any
    environment{
        SECRET_VAR = credentials('secret_text')
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages{
        stage('init'){
            steps{
                sh 'docker stop mynginx || true'
                sh 'docker rm mynginx || true'
            }
        }
        stage('build'){
            steps{
                sh 'docker run -d -p 80:80 --name mynginx nginx:latest'
                sh "docker exec mynginx sh -c 'echo \"hello jenkins ${SECRET_VAR}\" > /usr/share/nginx/html/index.html' "
            }
        }
        stage('push'){
            steps{
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password -stdin"
                sh "docker tag nginx jakepaulsmith/mynginx1:latest"
                sh "docker push jakepaulsmith/mynginx1:latest"
            }
        }
    }
}
