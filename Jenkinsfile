node {
    // Define variables for the Docker application, container name, and image name
    def app
    def containerName = 'computerhacking101_website'
    def imageName = 'computerhakcing101/computerhacking101website'

    // Set environment variables using credentials stored in Jenkins
    environment {
        PORTAINER_API_URL = credentials('portainer-api-url')
        PORTAINER_API_TOKEN = credentials('portainer-api-token')
    }

    // Clone the repository containing the Dockerfile and other necessary files
    stage('Clone repository') {
        checkout scm
    }

    // Build the Docker image using the Dockerfile
    stage('Build image') {
        app = docker.build(imageName)
    }

    // Push the Docker image to Docker Hub with two tags: the incremental build number and "latest"
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    // Recreate the Docker container using Portainer's API
    stages {
        stage('Recreate container') {
            steps {
                script {
                    // Retrieve the Portainer API URL and token from the environment variables
                    def portainerApiUrl = sh(script: "echo ${PORTAINER_API_URL}", returnStdout: true).trim()
                    def portainerApiToken = sh(script: "echo ${PORTAINER_API_TOKEN}", returnStdout: true).trim()
                    
                    // Define the request body for the API call
                    def requestBody = [action: 'recreate']

                    // Call the Portainer API to recreate the container
                    sh """
                        curl --request POST \
                            --url ${portainerApiUrl}/1/docker/containers/${containerName}/json \
                            --header 'Authorization: Bearer ${portainerApiToken}' \
                            --header 'Content-Type: application/json' \
                            --data '${requestBody}'
                    """
                }
            }
        }
    }
}