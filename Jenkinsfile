pipeline {
    agent { label 'docker-server1' }
    
    tools {nodejs "NodeJS-18-16-0"}

    stages {
        stage('Build') {
            steps {
                sh '''
                npm install'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.11.115:9000/ \
                -Dsonar.login=sqp_968e23faac3944b09522fe8a2294813537747870'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker tag simple-apps-pipeline dtosc/simple-apps-pipeline
                docker push dtosc/simple-apps-pipeline
                docker image prune -a -f
                '''
            }
        }
    }
}
