node {
    // Define variables for the Docker image and container names and sitemap
    def app
    def imageName = 'computerhacking101/computerhacking101website'
    def sitemapUrl = 'https://computerhacking101.com/sitemap.xml'

    // Set environment variables using credentials stored in Jenkins
    environment {
        CF_CACHE_TAGS = '*'
    }

    // Clone the source code repository to the workspace
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }

    // Build the Docker image
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build(imageName)
    }

    // Push the Docker image to the registry on Docker Hub
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

    // Purge the Cloudflare cache
    stage('Purge Cache') {
        // Make sure to add the required credentials in Jenkins before running this step
        withCredentials([string(credentialsId: 'CLOUDFLARE_ZONE_Id_CH101', variable: 'CF_ZONE_ID'),
                             string(credentialsId: 'CLOUDFLARE_API_TOKEN', variable: 'CF_API_TOKEN')]) {
            script {
                sh "curl -X POST \"https://api.cloudflare.com/client/v4/zones/${CF_ZONE_ID}/purge_cache\" \
                    -H \"Authorization: Bearer ${CF_API_TOKEN}\" \
                    -H \"Content-Type: application/json\" \
                    --data '{\"purge_everything\":true}'"
            }
        }
    }
    
    // Submit the sitemap to Google
    stage("Submit sitemap to google") {
        sh "curl -X POST \"https://www.google.com/ping?sitemap=${sitemapUrl}\""
    }
    
}
