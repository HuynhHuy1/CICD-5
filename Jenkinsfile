pipeline {

    agent any

    tools { 
        maven 'my-maven' 
    }
    // environment {
    //     MYSQL_ROOT_LOGIN = credentials('dockerhub')
    // }
    stages {

        stage('Build with Maven') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Packaging/Pushing imagae') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t huy21it490/democicd .'
                    sh 'docker push huy21it490/democicd'
                }
            }
        }


        stage('Deploy Spring Boot to Kubernetes') {
            steps {
                // echo 'Deploying and cleaning'
                // sh 'docker container rm -f democicd || echo "No such container"'
                // sh 'docker build -t democicd .'
                // sh 'docker container run -d --rm --name democicd -p 8081:8000 democicd' 
                script {
                    withKubeConfig([credentialsId: 'kubectl', serverUrl: 'http://34.143.232.173:6443']) {
                        sh 'kubectl apply -f deployment.yaml'
                    }
                }
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}