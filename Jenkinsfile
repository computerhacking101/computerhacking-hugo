node {
    def app
    def containerName = 'computerhacking101_new_website'
    def imageName = 'computerhakcing101/computerhacking101website'

     // Set environment variables using credentials stored in Jenkins
    environment {

        CF_CACHE_TAGS = '*'
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
    


stage('Purge Cache') {
            steps {
                withCredentials([string(credentialsId: 'CLOUDFLARE_ZONE_Id_CH101', variable: 'CF_ZONE_ID'),
                                 string(credentialsId: 'CLOUDFLARE_API_TOKEN', variable: 'CF_API_TOKEN')]) {
                    script {
                        sh "curl -X DELETE \"https://api.cloudflare.com/client/v4/zones/${CF_ZONE_ID}/purge_cache\" \
                             -H \"Authorization: Bearer ${CF_API_TOKEN}\" \
                             -H \"Content-Type: application/json\" \
                             --data '{\"purge_everything\":true}'"
                    }
                }
            }
        }
   // stage ("submit sitemap to google")
   //     sh """
    //        curl -X POST "https://www.google.com/ping?sitemap=https://computerhacking101.com/sitemap.xml"
     //            
      //  """

}
