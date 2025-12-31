pipeline {
    agent any

    
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

        stage('Test') {
                    steps {
                        sh """
                             mvn sonar:sonar \
                                 -Dsonar.host.url=http://localhost:9000 \
                                 -Dsonar.login=sqa_9eec0abf28efc3f3ea27bef01323132a6574f97d
                                               """
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