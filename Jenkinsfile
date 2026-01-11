pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ImaneHi/jenkins-tp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Test') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=todo-app \
                      -Dsonar.projectName=todo-app
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t jenkins-tp:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag jenkins-tp:latest your-dockerhub-username/jenkins-tp:latest
                        docker push your-dockerhub-username/jenkins-tp:latest
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8080:8080 --name jenkins-tp-container your-dockerhub-username/jenkins-tp:latest'
            }
        }
    }
}
