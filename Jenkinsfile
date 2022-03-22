pipeline {
    agent any
    tools {
      terraform 'terraform'
    }
    tools {
      maven 'maven3'
    }

    stages {
        stage ("Build backend") {
            steps {
                withMaven{
                    sh 'mvn clean install'
                }
            }
        }
        stage('Build Frontend') {
            steps { 
                sh 'npm install'
            }
        }
        stage('docker-compose') {
           steps {
            //   sh "docker-compose build"
              sh "docker-compose up -d"
           }
        }
    //     stage ("terraform init") {
    //         steps {
    //             sh 'terraform init'
    //         }
    //     }
    //     stage ("terraform fmt") {
    //         steps {
    //             sh 'terraform fmt'
    //         }
    //     }
    //     stage ("terraform validate") {
    //         steps {
    //             sh 'terraform validate'
    //         }
    //     }
    //     stage ("terrafrom plan") {
    //         steps {
    //             sh 'terraform plan '
    //         }
    //     }
    //     stage ("terraform apply") {
    //         steps {
    //             sh 'terraform apply --auto-approve'
    //         }
    //     }
       
    //     stage ('Build jar - Backend') {
    //         steps {
    //             sh 'mvn -f validator-backend/pom.xml clean install'         
    //         }
    //     }
    //     // Building Docker images
    //     stage('Building image') {
    //         steps{
    //             script {
    //             dockerImage = docker.build registry 
    //             }
    //         }
    //     }
    //     stage ('Docker Build and push to docker-hub') {
    //         steps{
    //                 // Build and push image with Jenkins' docker-plugin
    //                 withDockerServer([uri: "tcp://localhost:4243"]) {
    
    //                     withDockerRegistry([credentialsId: "fa32f95a-2d3e-4c7b-8f34-11bcc0191d70", url: "https://index.docker.io/v1/"]) {
    //                     image = docker.build("jaymezon/jumia_phone_validator", "jumia_phone_validator")
    //                     image.push()
                        
    //                 }
    //             }
    //         }
    //     }
    }
}