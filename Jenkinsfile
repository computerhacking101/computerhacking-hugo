node {
    def app
    def containerName = 'computerhacking101_new_website'
    def portainerApiUrl = 'https://myportainer.chrisallen.us/api/endpoints'
    def imageName = 'computerhakcing101/computerhacking101website'

     // Set environment variables using credentials stored in Jenkins
    environment {
        // PORTAINER_API_TOKEN = credentials('portainer-api-token')
       // PORTAINER_API_TOKEN = credentials('ptr_p15Tx5Mb97bkDE6sO45ATvTCisP9heWE8Hk4PYS2Y/M=')
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
    stage ("submit sitemap to google")
        sh """
            curl -X POST "https://www.google.com/ping?sitemap=https://computerhacking101.com/sitemap.xml"
                 
        """

}
