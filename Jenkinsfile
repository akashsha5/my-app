node{
   stage('SCM Checkout'){
     git 'https://github.com/akashsha5/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Build Docker Imager'){
   sh 'docker build -t akashshak/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u akashshak -p ${dockerPassword}"
    }
   sh 'docker push akashshak/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 52.66.247.237:8083"
   sh "docker tag akashshak/myweb:0.0.2 52.66.247.237:8083/damo:1.0.0"
   sh 'docker push 52.66.247.237:8083/damo:1.0.0'
   }	
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest akashshak/myweb:0.0.2' 
   }
}
}
