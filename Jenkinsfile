pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t jasonatkins/task1jenk .
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push jasonatkins/task1jenk
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
		    kubectl apply -f .
                '''
            }

        }

    }

}

