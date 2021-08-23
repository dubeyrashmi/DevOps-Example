node {
    // reference to maven
    // ** NOTE: This 'maven-3.5.2' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'MAVEN3'
    //def dockerHome = tool 'MyDocker'


    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)
 
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"

    jdk = tool name: 'JDK8'
      env.JAVA_HOME = "${jdk}"

      echo "jdk installation path is: ${jdk}"

      // next 2 are equivalents
      sh "${jdk}/bin/java -version"

      // note that simple quote strings are not evaluated by Groovy
      // substitution is done by shell script using environment
      sh '$JAVA_HOME/bin/java -version'

    stage('Initialize')
            {
                def dockerHome = tool 'MyDocker'
                //def mavenHome  = tool 'MyMaven'
                env.PATH = "${dockerHome}/bin:${env.PATH}"

            }
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/dubeyrashmi/DevOps-Example.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.5.2' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'MAVEN3'
    }    
  
    stage('Build Project') {


      // build project via maven
      sh "'${mvnHome}/bin/mvn' clean install"
    }
		
    stage('Build Docker Image') {
      // build docker image
      dockerImage = docker.build("devopsexample:${env.BUILD_NUMBER}")
    }
   
    stage('Deploy Docker Image'){
      
      // deploy docker image to nexus
		
      echo "Docker Image Tag Name: ${dockerImageTag}"
	  
	  //docker ps -q --filter "name=devopsexample" | grep -q . && docker stop devopsexample && docker rm -fv devopsexample
	   sh "docker stop devopsexample || true"
	   sh "docker rm devopsexample || true"

	  
	  sh "docker run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"

	  //sh "docker push http://0.0.0.0:8081/repository/maven-nexus-repo/devopsexample:${env.BUILD_NUMBER}"
	  sh "docker push http://0.0.0.0:8081/repository/nexus_docker_test/devopsexample:${env.BUILD_NUMBER}"
	  
	  // docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
      //    dockerImage.push("${env.BUILD_NUMBER}")
      //      dockerImage.push("latest")
      //  }
      
    }
}
