pipeline {
    agent {
        docker {
            // TODO build & push your Jenkins agent image, place the URL here
            image '700935310038.dkr.ecr.eu-west-2.amazonaws.com/eli-abukrat-jenkins-agent:latest'
            args  '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
    }
    environment {
        APP_NAME = "bot"
        APP_ENV = "prod"

    }
    parameters {
        string(name: 'BOT_IMAGE_NAME')
    }

     stages {

        stage('yaml build'){
            steps {
                sh "sed -i 's|BOT_IMAGE|$BOT_IMAGE_NAME|g' infra/k8s/bot.yaml"

            }
        }
        stage('Bot Deploy') {
            steps {

                withCredentials([
                    file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')
                ]) {

                    sh '''

                    # apply the configurations to k8s cluster

                     kubectl apply --kubeconfig ${KUBECONFIG} -f infra/k8s/bot.yaml --namespace prod

                    '''
                }
            }
        }
    }

}