pipeline {
    agent any
environment {
    YOUR_NAME=credentials("YOUR_NAME")
}
   
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t jasonatkins/task1jenk:v${BUILD_NUMBER} .
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push jasonatkins/task1jenk:v${BUILD_NUMBER}
                '''
            }

        }
        stage('Staging Deploy')
        {
            steps{
                sh '''
                    kubectl apply -f nginx-configmap.yaml --namespace staging
                    sed -e 's,{{YOUR_NAME}},'${YOUR_NAME}',g;' -e 's,{{version}},'${BUILD_NUMBER}',g;' app-manifest.yaml | kubectl apply -f - --namespace staging
                    kubectl apply -f nginx-pod.yaml --namespace staging
                '''
            }
        }
        stage('Quality Test')
        {
            steps{
                sh '''
                    sleep 50 
                    export STAGING_IP=\$(kubectl get svc -o json --namespace staging | jq '.items[] | select(.metadata.name == "nginx") | .status.loadBalancer.ingress[0].ip' | tr -d '"')
                    pip3 install requests
                    python3 test-app.py
                '''
            }
        }
        stage('Prod Deploy') {
            steps {
                sh '''
                    kubectl apply -f nginx-configmap.yaml --namespace production
                    sed -e 's,{{yourname}},'"${YOUR_NAME}"',g;'  -e 's,{{version}},'${BUILD_NUMBER}',g;' app-manifest.yaml | kubectl apply -f - --namespace production
                    kubectl apply -f nginx-pod.yaml --namespace production
                    sleep 60
                    kubectl get services --namespace production
                '''
            }

        }

    }

}

