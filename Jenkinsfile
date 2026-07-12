pipeline {
    agent { label 'docker-server1' }
    tools {nodejs "NodeJS-18-16-0"}

environment {
    NAMEAPPS = 'simple-apps-pipeline-apps'
    SONARHOST = 'http://172.23.11.115:9000/' 
    TOKENSONAR = 'sqp_968e23faac3944b09522fe8a2294813537747870'
    CTREG = 'dtosc'
} 
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
                -Dsonar.host.url=${SONARHOST} \
                -Dsonar.token=${TOKENSONAR}'''
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
        stage('Push and Clean Images') {
            steps {
                sh '''
                docker tag ${NAMEAPPS} ${CTREG}/${NAMEAPPS}
                docker push ${CTREG}/${NAMEAPPS}
                docker image prune -a -f
                '''
            }
        }
    }
}
