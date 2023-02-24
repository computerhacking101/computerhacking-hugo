node {
    // Define variables for the Docker application, container name, and image name
    def app
    def containerName = 'computerhacking101_website'
    def imageName = 'computerhakcing101/computerhacking101website'
    def portainerApiUrl = "https://myportainer.chrisallen.us/api/endpoints"

    // Set environment variables using credentials stored in Jenkins
    environment {
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
    
    
}
