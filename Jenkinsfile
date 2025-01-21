// @Library('Shared')_
// pipeline{
//     agent { label 'dev-server'}
    
//     stages{
//         stage("Code clone"){
//             steps{
//                 sh "whoami"
//             clone("https://github.com/LondheShubham153/django-notes-app.git","main")
//             }
//         }
//         stage("Code Build"){
//             steps{
//             dockerbuild("notes-app","latest")
//             }
//         }
//         stage("Push to DockerHub"){
//             steps{
//                 dockerpush("dockerHubCreds","notes-app","latest")
//             }
//         }
//         stage("Deploy"){
//             steps{
//                 deploy()
//             }
//         }
        
//     }
// }

@Library('shared') _
pipeline {
    agent { label "Abinash" }
    
    stages {
        stage("SharedLibrary") {
            steps {
                hello()
               
            }
        }
      stage("Code") {
            steps {
                echo "Cloning the project"
                git url: "https://github.com/Bickyyadav/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the project"
                sh "docker  build -t node-app:latest ."
                echo "Docker build completed successfully"
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhubcredential", usernameVariable: "dockerHubUser", passwordVariable: "dockerHubPass")]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                    sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the application"
                sh "docker run -d -p 8000:8000 node-app:latest"
            }
        }
    }
}
