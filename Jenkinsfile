pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/shahid-devops/tomcat.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t mavenapp:latest .' 
                sh 'docker tag mavenapp shahid9741/mavenapp:latest'
                //sh 'docker tag mavenapp shahid9741/mavenapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "docker", url: "" ]) {
          sh  'docker push shahid9741/mavenapp:latest'
        //  sh  'docker push shahid9741/mavenapp:latest:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8081 shahid9741/mavenapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@192.168.0.4 run -d -p 8003:8081 shahid9741/mavenapp"
 
            }
        }
    }
	}
    
