pipeline {
    agent any
    environment {
        BUILD_VER = 'PROJECT-ID'
        LOCATION = 'CLUSTER-LOCATION'
        CREDENTIALS_ID = 'gke'
        CLUSTER_NAME_TEST = 'CLUSTER-NAME-1'
        CLUSTER_NAME_PROD = 'CLUSTER-NAME-2'          
    }
    
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
        /*stage('Deploy to GKE test cluster') {
            steps{
                sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_TEST, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
        stage('Deploy to GKE production cluster') {
            steps{
                input message:"Proceed with final deployment?"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_PROD, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }*/  
    }    
}
