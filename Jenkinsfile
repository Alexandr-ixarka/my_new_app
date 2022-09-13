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

	
	
	stage('Upload to S3') {
        steps{
            script {

                dir(''){

                    pwd(); //Log current directory

                    withAWS(region:'us-east-1',credentials:'jenkinsawsbucketkozich') {

                        def identity=awsIdentity();//Log AWS credentials

                        // Upload files from working directory '' in your project workspace
                        s3Upload(bucket:"jenkinsawsbucketkozich", workingDir:'', includePathPattern:'**/*');
                        // invalidate CloudFront distribution
                       // cfInvalidate(distribution:'E152QNNVYS423', paths:['/*'])
                    }

                };
            }
        }
    }

  }
	
	
	
	/*
	stage('Upload') {

       /* dir('/var/lib/jenkins/workspace/new-item-pipeline/'){*/

          /*  withAWS(region:'us-east-1',credentials:'jenkinsawsbucketkozich') {

                 def identity=awsIdentity();//Log AWS credentials

                // Upload files from working directory 'dist' in your project workspace
                s3Upload(bucket:"us-east-1", workingDir:'dist', includePathPattern:'**///*//');
       //     }

        //};
    ///}
	
	

//}
