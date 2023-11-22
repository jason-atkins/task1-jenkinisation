pipeline {
    agent any
environment {
    YOUR_NAME=credentials("YOUR_NAME")
}
   
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
            sed -e 's,{{yourname}},'${YOUR_NAME}',g;' your-name-secret.yaml | kubectl apply -f -
		    kubectl apply -f nginx-configmap.yaml
            kubectl apply -f task1-app-manifest.yaml
            kubectl apply -f nginx-manifest.yaml
            sleep 60
            kubectl get services
                '''
            }

        }

    }

}

