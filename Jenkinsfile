pipeline {
    agent any
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('var') {
            steps {
               sh "printenv | sort"
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("myregistry.com/root/demo/hello:jenkins-${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://myregistry.com', 'registry-auth') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }       
        stage('Deploy to kubernetes cluster') {
            steps{
                sh "kubectl apply -y deploy-app.yml"
            }
        }
    }    
}
