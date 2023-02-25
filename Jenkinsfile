node {
    def app
    def containerName = 'computerhacking101_new_website'
    def imageName = 'computerhakcing101/computerhacking101website'

     // Set environment variables using credentials stored in Jenkins
    environment {
        CLOUDFLARE_API_TOKEN = credentials('cloudflare-api-token')
        CLOUDFLARE_ZONE_TOKEN = credentials('cloudflare-zone-token-ch101')

       
    }

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
        stage ("tell cloudflare to dump it cache of your website")
            // Retrieve the cloudflare API token from the environment variables
             def cloudflareApiToken = sh(script: "echo ${CLOUDFLARE_API_TOKEN}", returnStdout: true).trim()
            //Retrieve the cloudflare zone for your domain from the environment variables
            //def cloudflareZoneToken = sh(script: "echo ${CLOUDFLARE_ZONE_TOKEN}", returnStdout: true).trim()

        sh """
            curl -X POST "https://api.cloudflare.com/client/v4/zones/09d7e4e4e1c4c6ca9d00ce90ea561a45/purge_cache" \
            -H "X-Auth-Email: mcncyo@gmail.com" \
            -H "X-Auth-Key: ${cloudflareApiToken}" \
            -H "Content-Type: application/json" \
            --data '{"purge_everything":true}'
            
                 
        """
    stage ("submit sitemap to google")
        sh """
            curl -X POST "https://www.google.com/ping?sitemap=https://computerhacking101.com/sitemap.xml"
                 
        """

}
