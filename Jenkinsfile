pipeline {
    agent any

    stages {

        stage('Git clone') {
            steps {
                echo 'Clone'
                git branch: 'main', url: 'https://github.com/Balumidikondla/Balumidikondla-project-1-new_project_with_pyt.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python3 -m pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t balumidikondla/python-app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                        sh 'docker login -u balumidikondla -p ${dockerhub}'
                    }

                    sh 'docker push balumidikondla/python-app:latest'
                    echo 'pushed to docker hub'
                }
            }
        }

        stage('Deploy') {
    steps {
        sh '''
        echo "Stopping old container if exists..."
        docker rm -f python-app || true

        echo "Running new container..."
        docker run -d --name python-app -p 5000:5000 balumidikondla/python-app:latest
        '''
    }
}
}
}

