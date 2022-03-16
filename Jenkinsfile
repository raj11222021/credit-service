node {
  
   def MAVEN_HOME = tool "mymaven"
   env.PATH = "${env.PATH}:${MAVEN_HOME}/bin"
  
  
  stage('checkout') {
    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/raj11222021/credit-service.git']]])    
  }    
  
  stage('Initial Setup'){
    sh 'mvn clean compile' 
  }
  
}
