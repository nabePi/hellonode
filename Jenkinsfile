node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("getintodevops/hellonode")
    }

    // stage('Test image') {
    //     /* Ideally, we would run a test framework against our image.
    //      * For this example, we're using a Volkswagen-type approach ;-) */

    //     app.inside {
    //         sh 'echo "Tests passed"'
    //     }
    // }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('http://docker.staging.ktbs.io/', 'docker-registry-ktbs.io') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Run image') {
        sh "docker login https://docker.staging.ktbs.io -u kitabisa -p kitabisa#12342019"
        sh "docker pull getintodevops/hellonode"
        sh "docker run -d --rm -p 8000:8000 --name hellonode getintodevops/hellonode:latest"
        echo "Application started on port: 8000 (http)"
    }
}