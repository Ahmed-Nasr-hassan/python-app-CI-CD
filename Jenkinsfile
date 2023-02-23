pipeline {
    agent any
      environment {
        Creation_Date = sh(script: "date +'%b_%d_%H_%M'", returnStdout: true).trim()
      }
    stages {
        stage('CI') {
            steps {
                git url: 'https://github.com/Ahmed-Nasr-hassan/python-app-CI-CD', branch: 'main'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'MY_PASS', usernameVariable: 'MY_USER' )]) {
                sh '''
                  Creation_Date=`date +'%b_%d_%H_%M'`
                  echo username is ${MY_USER}
                  docker login -u ${MY_USER} -p ${MY_PASS}
                  docker build -t ahmednasrhassan/python-app:${Creation_Date} .
                  docker push ahmednasrhassan/python-app:${Creation_Date}
                  echo $Creation_Date
                '''
              }

            }
        }
        
        stage('CD - inside k8s') {
            steps {
                sh '''
                  kubectl apply -f ./k8s-yaml-files/env-configmap.yaml
                  // kubectl apply -f ./k8s-yaml-files/deployment-devops-challenge.yaml
                  envsubst < ./k8s-yaml-files/deployment-devops-challenge.yaml | kubectl apply -f -
                '''
                }
            }
            
        }


    }
