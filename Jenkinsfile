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
    }
}
