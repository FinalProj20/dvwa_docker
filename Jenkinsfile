node {

    checkout scm

    docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {

        def customImage = docker.build("FinalProj20/dvwa_docker")

        /* Push the container to the custom Registry */
        customImage.push()
    }
}
