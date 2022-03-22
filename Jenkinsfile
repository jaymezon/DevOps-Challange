pipeline {
    agent any
    // tools {
    //   terraform 'terraform'
    // }
    // tools {
    //   maven 'maven3'
    // }
    environment{
        dockerImage = ''
        registry = 'jaymezon/validator-backend-image'
        registry = 'jaymezon/validator-frontend-image'
        registryCredential = 'dockerhub_id'
    }
    stages {
        stage ('Build jar - Backend') {
            steps {
                sh 'mvn -f validator-backend/pom.xml clean install'         
            }
        }
        stage ('Build Frontend') {
            steps {
                sh 'npm install' 
                sh 'npm run build'      
            }
        }
        stage('Build Docker image') {
           steps {
               script{
                  dockerImage = docker.build registry 
               }           
           }
        }
          // Uploading Docker images into Docker Hub
        stage('Upload Image') {
            steps{    
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }
        // Stopping Docker containers for cleaner Docker run
        stage('docker stop container') {
            steps {
                sh 'docker ps -f name=vidaaltor-backend-image -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -fname=validator-backend-image -q | xargs -r docker container rm'
                 sh 'docker ps -f name=validator-frontend-image -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -fname=validator-frontend-image -q | xargs -r docker container rm'
            }
        }
        // Stopping Docker containers for cleaner Docker run
        stage('docker stop container') {
            steps {
                script{
                    dockerImage.run("-p 8080:8080 --rm vidaaltor-backend-image")
                    dockerImage.run("-p 8081:8081 --rm validator-frontend-image")
                }
                // sh 'docker ps -f name=vidaaltor-backend-image -q | xargs --no-run-if-empty docker container stop'
                // sh 'docker container ls -a -fname=vidaaltor-backend-image -q | xargs -r docker container rm'
                // sh 'docker ps -f name=validator-frontend-image -q | xargs --no-run-if-empty docker container stop'
                // sh 'docker container ls -a -fname=validator-frontend-image -q | xargs -r docker container rm'
            }
        }
        // stage('docker-compose') {
        //    steps {
        //     //   sh "docker-compose build"
        //       sh "docker-compose up -d"
        //    }
        // }

        // stage ("Build backend") {
        //     steps {
        //         withMaven(globalMavenSettingsConfig: 'null', jdk: 'null', maven: 'maven3', mavenSettingsConfig: 'null') {
        //             // some block
        //         }
        //     }
        // }
        // stage('Build Frontend') {
        //     steps { 
        //         sh 'npm install'
        //     }
        // }
        
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