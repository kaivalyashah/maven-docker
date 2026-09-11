pipeline {
    agent any
    triggers {
        pollSCM '* * * * *'
    }

    stages {
        stage("Maven-Build") {
            steps {
                sh '''
                
                cd my-maven-docker-project
                mvn clean install

                '''
            }
        }

        stage("Docker-Build") {
            steps {
                sh '''
                cd my-maven-docker-project
                echo "Building Docker"
                docker build -t java-image:v1 .
                '''
                
            }}
	stage('ECR-Push') {
    steps {
        sh '''
            aws ecr get-login-password --region us-east-2 | \
            docker login --username AWS --password-stdin \
            605134445572.dkr.ecr.us-east-2.amazonaws.com

            docker tag java-image:v1 \
            605134445572.dkr.ecr.us-east-2.amazonaws.com/maven-project:v1

            docker push \
            605134445572.dkr.ecr.us-east-2.amazonaws.com/maven-project:v1
        '''
    }

        }
    }
}
