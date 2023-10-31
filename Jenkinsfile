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


        // stage('Deploy MySQL to DEV') {
        //     steps {
        //         echo 'Deploying and cleaning'
        //         sh 'docker image pull mysql:8.0'
        //         sh 'docker network create dev || echo "this network exists"'
        //         sh 'docker container stop khalid-mysql || echo "this container does not exist" '
        //         sh 'echo y | docker container prune '
        //         sh 'docker volume rm khalid-mysql-data || echo "no volume"'

        //         sh "docker run --name khalid-mysql --rm --network dev -v khalid-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_LOGIN_PSW} -e MYSQL_DATABASE=db_example  -d mysql:8.0 "
        //         sh 'sleep 20'
        //         sh "docker exec -i khalid-mysql mysql --user=root --password=${MYSQL_ROOT_LOGIN_PSW} < script"
        //     }
        // }

        stage('Deploy Spring Boot to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker container rm -f democicd || echo "No such container"'
                sh 'docker build -t democicd .'
                sh 'docker container run -d --rm --name democicd -p 8081:8000 democicd' 
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