pipeline {
    agent {
        docker {

            image '700935310038.dkr.ecr.eu-west-2.amazonaws.com/eli-abukrat-jenkins-agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
   options{
     timestamps()
    }


    environment {
        REPO_URL = '700935310038.dkr.ecr.eu-west-2.amazonaws.com'
        IMAGE_NAME ='ci-cd-bot-prod'
        IMAGE_TAG = '${BUILD_NUMBER}'
   }


    stages {
        stage('Build') {
            steps {
                sh '''

                 aws ecr get-login-password --region eu-west-2 | docker login --username AWS --password-stdin 700935310038.dkr.ecr.eu-west-2.amazonaws.com
                 docker build -t ${IMAGE_NAME} -f bot/Dockerfile .
                 docker tag ${IMAGE_NAME} ${REPO_URL}/${IMAGE_NAME}:${BUILD_NUMBER}

                 docker push ${REPO_URL}/${IMAGE_NAME}:${BUILD_NUMBER}

                '''


            }

            post {
               always {
                   sh 'docker image prune -a --filter "until=240h" --force'
               }
            }

        }


    }
}