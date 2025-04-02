pipeline {
    agent any 
    environment{
        registryUrlBase = "https://ghcr.io"
        registryUrl = "ghcr.io/DevOpsNazim/Jenkins_Pipeline_CICD"
        registryCredentialsId = 'github-token'
        commitID = sh(script: 'git rev-parse --short HEAD', returnStdout:true).trim()
        containerName = "nazim-web"
    }
    stages {
        stage('Build and Push Image') { 
            steps {
                // 
                sh"whoami"
                script {
                    docker.withRegistry(registryUrlBase, registryCredentialsId) {
                        dockerImage = docker.build("$registryUrl:$commitID","./JenkinsCICD")
			            dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh "docker stop $containerName"
                sh "docker rm $containerName"
                sh "docker run -d -p 8080:8080 --name $containerName $registryUrl:$commitID"
            }
        }
        stage('Clean Up') { 
            steps {
                sh "docker rmi --force $registryUrl:$commitID"
            }
        }
    }
}
