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
                    myapp = docker.build("myregistry.com/root/cicd-pipeline/hello:jenkins-${env.BUILD_ID}")
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
                sh "sed -i 's/hello:jenkins-ENV_BUILD_ID/hello:${env.BUILD_ID}/g' deploy-app.yml"
                sh "kubectl apply -f deploy-app.yml"
            }
        }
    }    
}
