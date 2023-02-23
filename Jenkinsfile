pipeline {
    agent any

    environment {
        // Initialize APP_version with default value of 1.0 if it does not exist
        APP_version = "${env.APP_version ?: '1.0'}"
    }

    stages {
        stage('CI') {

                // Increment the APP_version environment variable by 0.1
                def (major, minor) = APP_version.tokenize('.')
                minor = (minor as Float) + 0.1
                APP_version = "${major}.${minor}"

                // Set the updated APP_version variable as an environment variable
                environment {
                    name 'APP_version'
                    value APP_version

            steps {
                git url: 'https://github.com/Ahmed-Nasr-hassan/python-app-CI-CD', branch: 'main'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'MY_PASS', usernameVariable: 'MY_USER' )]) {
                    sh '''
                        echo username is ${MY_USER}
                        docker login -u ${MY_USER} -p ${MY_PASS}
                        docker build -t ahmednasrhassan/python-app:${APP_version} .
                        docker push ahmednasrhassan/python-app:${APP_version}
                    '''
                }

                }
            }
        }

        stage('CD - inside k8s') {
            steps {
                sh '''
                    kubectl apply -f ./k8s-yaml-files/env-configmap.yaml
                    kubectl apply -f ./k8s-yaml-files/deployment-devops-challenge.yaml --namespace=default
                    kubectl set image deployment/devops-challenge devops-challenge=ahmednasrhassan/python-app:${APP_version}
                '''
            }
        }
    }
}
