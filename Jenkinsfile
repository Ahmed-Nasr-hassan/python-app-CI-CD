pipeline {
    agent any

    environment {
        // Initialize APP_version with default value of 1.0 if it does not exist
        APP_version = env.APP_version ?: '1.0'
    }

    stages {
        stage('CI') {
            steps {
                git url: 'https://github.com/Ahmed-Nasr-hassan/python-app-CI-CD', branch: 'main'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'MY_PASS', usernameVariable: 'MY_USER' )]) {
                sh '''
                  echo username is ${MY_USER}
                  docker login -u ${MY_USER} -p ${MY_PASS}
                  docker build -t ahmednasrhassan/python-app .
                  docker push ahmednasrhassan/python-app
                  echo $APP_version
                '''
              }

            }
        }
        
        stage('CD - inside k8s') {
            steps {
                sh '''
                  kubectl apply -f ./k8s-yaml-files/env-configmap.yaml
                  kubectl delete -f ./k8s-yaml-files/deployment-devops-challenge.yaml
                  kubectl apply -f ./k8s-yaml-files/deployment-devops-challenge.yaml
                '''
                }
            }
            
        }


    }
