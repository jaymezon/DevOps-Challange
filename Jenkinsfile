pipeline {
    agent any
    
    tools {
        terraform 'terraform'
    }
    tools {
        maven 'maven3'
    }
    stages {
        stage ("checkout from GIT") {
            steps {
                git branch: 'main', credentialsId: 'cde06f21-9ae7-4081-a549-f7bdb515dc6f', url: 'https://github.com/codepipe/tff.git'
            }
        }
        stage ("Build Backend") {
            steps {
                git branch: 'main', credentialsId: 'cde06f21-9ae7-4081-a549-f7bdb515dc6f', url: 'https://github.com/codepipe/tff.git'
            }
        }
        stage ("Build Frontend") {
            steps {
                git branch: 'main', credentialsId: 'cde06f21-9ae7-4081-a549-f7bdb515dc6f', url: 'https://github.com/codepipe/tff.git'
            }
        }
        stage ("Dockerize Frontend") {
            steps {
                git branch: 'main', credentialsId: 'cde06f21-9ae7-4081-a549-f7bdb515dc6f', url: 'https://github.com/codepipe/tff.git'
            }
        }
        stage ("Dockerize Backend") {
            steps {
                git branch: 'main', credentialsId: 'cde06f21-9ae7-4081-a549-f7bdb515dc6f', url: 'https://github.com/codepipe/tff.git'
            }
        }
        stage ("terraform init") {
            steps {
                sh 'terraform init'
            }
        }
        stage ("terraform fmt") {
            steps {
                sh 'terraform fmt'
            }
        }
        stage ("terraform validate") {
            steps {
                sh 'terraform validate'
            }
        }
        stage ("terrafrom plan") {
            steps {
                sh 'terraform plan '
            }
        }
        stage ("terraform apply") {
            steps {
                sh 'terraform apply --auto-approve'
            }
        }
    }
}


// ---------------------

pipeline {
    agent any
    tools {
        maven 'Maven3'
    }    
    environment {
        registry = "account_id.dkr.ecr.us-east-2.amazonaws.com/my-docker-repo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/akannan1087/springboot-app']]])     
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean install'           
                }
        }
        stage ('Build') {
            steps {
                sh 'mvn -f jumia_phone_validator/pom.xml clean install'         
                }
        }
    // Building Docker images
        stage('Building image') {
        steps{
            script {
            dockerImage = docker.build registry 
            }
        }
        }
        stage ('Docker Build') {
            steps{
                // Build and push image with Jenkins' docker-plugin
                withDockerServer([uri: "tcp://localhost:4243"]) {

                    withDockerRegistry([credentialsId: "fa32f95a-2d3e-4c7b-8f34-11bcc0191d70", url: "https://index.docker.io/v1/"]) {
                    image = docker.build("ananthkannan/mywebapp", "MyAwesomeApp")
                    image.push()
                    
                    }
            }
        }
        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps{  
                script {
                        sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin account_id.dkr.ecr.us-east-2.amazonaws.com'
                        sh 'docker push account_id.dkr.ecr.us-east-2.amazonaws.com/my-docker-repo:latest'
                }
            }
        }

       stage('K8S Deploy') {
            steps{   
                script {
                    withKubeConfig([credentialsId: 'K8S', serverUrl: '']) {
                    sh ('kubectl apply -f  eks-deploy-k8s.yaml')
                    }
                }
            }
       }
    }
}

// akannan1087/springboot-app