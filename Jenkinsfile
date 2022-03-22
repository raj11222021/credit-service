podTemplate(cloud: 'kubernetes',label: 'kubernetes',
            containers: [
                    containerTemplate(name: 'podman', image: 'quay.io/containers/podman', privileged: true, command: 'cat', ttyEnabled: true)
					
            ]) 
{
node {
  
   def MAVEN_HOME = tool "mymaven"
   env.PATH = "${env.PATH}:${MAVEN_HOME}/bin" 
  
  stage('checkout') {
    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/raj11222021/credit-service.git']]])    
  }    
  
  stage('Initial Setup'){
    sh 'mvn clean compile' 
  }
  
  stage('Unit Testing'){
    sh 'mvn test' 
  }
  
  stage('Code Quality Analysis'){
    
    withSonarQubeEnv('mysonar') 
	    	{
                 sh 'mvn sonar:sonar -Dsonar.organization=myorg11222021 -Dsonar.projectKey=rd-credit-service'		
    		}
	stash includes: '*', name: 'myproject'
  }
}
	
node('kubernetes'){
   container('podman') {
	stage('Image Build'){
	   unstash 'myproject'
	   sh 'podman image build -t credit-service .'	
	    withCredentials([usernamePassword(credentialsId: 'dockerid', passwordVariable: 'PWDD', usernameVariable: 'USER')]) {
           			 sh 'docker login -u=$USER -p=$PWDD'
			
			
		}
		sh 'docker push raj11222021/creditservice:${TIME}'
   }
}
  
}
}
