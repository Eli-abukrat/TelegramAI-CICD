pipeline {
    agent {
        docker {
            // TODO build & push your Jenkins agent image, place the URL here
            image '700935310038.dkr.ecr.eu-west-2.amazonaws.com/eli-abukrat-jenkins-agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        APP_ENV = "dev"
    }

    parameters {
        string(name: 'BOT_IMAGE_NAME')
    }

    stages {
        stage('Bot Deploy') {
            steps {
             script {
                    env.BOT_IMAGE_NAME = env.BOT_IMAGE_NAME.toLowerCase()
                }
                withCredentials([
                    file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')
                ]) {
                    sh '''
                    sed "s|BOT_IMAGE_NAME|${BOT_IMAGE_NAME}|g" infra/k8s/bot.yaml
                    # apply the configurations to k8s cluster

                     kubectl apply --kubeconfig ${KUBECONFIG} -f infra/k8s/bot.yaml --namespace dev

                    '''
                }
            }
        }
    }
}