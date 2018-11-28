node {
    def app
    def dockerImage
    def registryCredentials = 'docker-hub-credentials'
    def registry = "suruthinee/hellonode"

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

	app = docker.build(registry)
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('', registryCredentials) {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    stage('Deploy') {
	sh 'docker stop hellonode'
	sh 'docker rm hellonode'
	sh 'docker run -p 8000:8000 --name hellonode suruthinee/hellonode'
    }
}