node {
    def app
    def containerName = 'computerhacking101_website'
    def portainerApiUrl = 'https://myportainer.chrisallen.us/api/endpoints'
    def imageName = 'computerhakcing101/computerhacking101website'

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("computerhacking101/computerhacking101website")
    }



    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }



    stage('Recreate container') {
        
            script {
                // Retrieve the Portainer API token from the environment variables
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
