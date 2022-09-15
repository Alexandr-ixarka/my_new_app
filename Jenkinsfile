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

	
	
	
	
	 stage('artifacts to s3') {
      try {
      // you need cloudbees aws credentials
      withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-key', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
       //  sh "aws s3 ls"
     //    sh "aws s3 cp addressbook_main/target/addressbook.war s3://cloudyeticicd/"
         }
      } catch(err) {
         sh "echo error in sending artifacts to s3"
      }
   }
	
	
	
	
	
	
	stage('Upload to S3') {
        steps{
            script {

                dir(''){

                    pwd(); //Log current directory

                    withAWS(region:'us-east-1',credentials:'aws-key') {

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
	
	
	
	
