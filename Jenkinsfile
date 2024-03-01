pipeline {
    agent any
    environment{
        SECRET_VAR = credentials('secret_text')
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages{
        stage('init'){
            steps{
                sh 'docker stop mynginxlab5 || true'
                sh 'docker rm mynginxlab5 || true'
            }
        }
        stage('build'){
            steps{
                sh 'docker run -d -p 80:80 --name mynginxlab5 nginx:latest'
                sh "docker exec mynginxlab5 sh -c 'echo \"hello jenkins ${SECRET_VAR}\" > /usr/share/nginx/html/index.html' "
            }
        }
        stage('push'){
            steps{
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "docker tag mynginxlab5 jakepaulsmith/mynginxlab5:latest"
                sh "docker push jakepaulsmith/mynginxlab5:latest"
            }
        }
    }
}
