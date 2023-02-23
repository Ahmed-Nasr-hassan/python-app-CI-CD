pipeline {
    agent {label 'master'}

    stages {
        stage('CI') {
            steps {
                git url: 'https://github.com/mahmoud254/jenkins_nodejs_example', branch: 'main'
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'MY_PASS', usernameVariable: 'MY_USER' )]) {
                sh '''
                  echo username is ${MY_USER}
                  docker login -u ${MY_USER} -p ${MY_PASS}
                  docker build -t ahmednasrhassan/python-app .
                  docker push ahmednasrhassan/python-app
                '''
              }

            }
        }
        
        stage('CD - inside k8s') {
            steps {
                sh '''
                  kubectl apply -f ./k8s-yaml-files/env-configmap.yaml
                  kubectl apply -f ./k8s-yaml-files/deployment-devops-challenge.yaml
                '''
                }
            }
            
        }


    }
}
