pipeline {
    agent any

    parameters {
        string(name: 'TAGNAME', defaultValue: 'latest', description: 'Docker image tag')
        string(name: 'ARGOCD_SERVER', defaultValue: 'http://37.27.70.42:30515/', description: 'ArgoCD server address')
        string(name: 'APP_NAME', defaultValue: 'jenkins-argocd', description: 'Application name in ArgoCD')
    }

    stages {
        stage('Docker build') {
            steps {
                sh "docker image build -t prajwlas0209/jenkinstest:${TAGNAME} ."
            }
        }
        stage('Docker push') {
            steps {
                sh "docker push prajwlas0209/jenkinstest:${TAGNAME}"
            }
        }
        stage('Argocd Deploy') {
            steps {
                sh """
                    argocd --grpc-web app sync $APP_NAME --force --server $ARGOCD_SERVER
                    argocd --grpc-web app wait $APP_NAME --timeout 600 --server $ARGOCD_SERVER
                """
            }
        }
    }
}
