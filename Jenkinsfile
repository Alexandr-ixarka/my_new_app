node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("q2w3e4r5/docker101tutorial1")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
 
   }

stage("Upload"){
 steps{
 withAWS(region:("us-east-1"), credentials:("jenkinsawsbucketkozich"){
 s3Upload(file:("*/*.js"), bucket:("jenkinsawsbucketkozich"), path:("/bucket/*.js"))
 }
 }
 }

}
