@Library("Shared") _
pipeline {
    agent { label "worker" }

    stages {
        stage("Hello") {
            steps {
                script {
                    hello()
                }
            }
        }

        stage("Code") {
            steps {
                script {
                    clone("https://github.com/kumawat2222/django-notes-app.git","main")
                }
            }
        }

        stage("Build"){
            steps{
                script{
                    docker_build("notes-app", "latest", "kailash2222")
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    docker_push("notes-app", "latest", "kailash2222")
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "This is deploying the code..."
                // Stop and remove any existing container to avoid conflicts
                sh "docker stop notes-app || true && docker rm notes-app || true"
                // Run the Docker container
                sh "docker run -d -p 8000:8000 --name notes-app notes-app:latest"
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for errors."
        }
    }
}
