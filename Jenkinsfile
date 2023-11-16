pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t jasonatkins/task1jenk .
                docker build -t jasonatkins/task1-nginx nginx
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push jasonatkins/task1jenk
                docker push jasonatkins/task1-nginx
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                    ssh jenkins@jason-deploy <<EOF
                    docker network rm task1-net && echo "task1-net removed" || echo "task1-net did not exist"
                    docker network create task1-net
                    docker stop nginx1 && echo "Stopped nginx1" || echo "nginx1 is not running"
                    docker rm nginx1 && echo "removed nginx1" || echo "nginx1 does not exist"
                    docker stop flask-app && echo "Stopped flask-app" || echo "flask-app is not running"
                    docker stop flask-app2 && echo "Stopped flask-app2" || echo "flask-app2 is not running"
                    docker stop flask-app3 && echo "Stopped flask-app3" || echo "flask-app3 is not running"
                    docker rm flask-app && echo "removed flask-app" || echo "flask-app does not exist"
                    docker rm flask-app2 && echo "removed flask-app2" || echo "flask-app2 does not exist"
                    docker rm flask-app3 && echo "removed flask-app3" || echo "flask-app3 does not exist"
                    docker run -d  --name flask-app --network task1-net jasonatkins/task1jenk
                    docker run -d  --name flask-app2 --network task1-net jasonatkins/task1jenk
                    docker run -d  --name flask-app3 --network task1-net jasonatkins/task1jenk
                    docker run -d --name nginx --network task1-net -p 80:80 jasonatkins/task1-nginx
                '''
            }

        }

    }

}

