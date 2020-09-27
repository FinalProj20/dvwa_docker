node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("finalproj20/dvwa_docker")
    }

    stage('Aqua Scan') {
            aquaMicroscanner imageName: 'finalproj20/dvwa_docker', notCompliesCmd: '', onDisallowed: 'ignore', outputFormat: 'html'
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
    stage('analyze') {
        def imageLine = 'finalproj20/dvwa_docker'
        writeFile file: 'anchore_images', text: imageLine
        anchore name: 'anchore_images'
    }
    
}
