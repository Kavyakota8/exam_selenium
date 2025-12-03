pipeline {
    agent any
    stages {
        stage('Run Selenium Tests with pytest') {
            steps {
                    echo "Running Selenium Tests using pytest" 
                    bat '"C:/Users/Kota Kavya Sri/AppData/Local/Programs/Python/Python38/python.exe" -m pip install -r requirements.txt'
 
                    bat 'start /B python app.py' 
                    bat 'ping 127.0.0.1 -n 5 > nul' 
                    bat 'python -m pytest -v'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t formapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                 bat 'docker login -u kavyakota18 -p Bkt@kota18'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag formapp:v1 kavyakota18/app:v1"               
                    
                bat "docker push kavyakota18/app:v1"
                
            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                    // apply deployment & service 
                    bat 'kubectl apply -f deployment.yaml --validate=false' 
                    bat 'kubectl apply -f service.yaml' 
            } 
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}

