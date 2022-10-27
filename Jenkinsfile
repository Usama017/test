pipeline {
    agent any
    tools {
	maven 'maven'
}	
    stages {
        stage("build jar") {
            steps {
		    
                echo "building app version"
		sh 'mvn package'
		    
	    }                        
          }
    	stage("build image") {
            steps {
		    
                echo "building docker image"
		withCredentials([usernamePassword(credentialsId: 'docker-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
			  
		    sh 'docker build -t usama07/demo-app:ver-1.2 .'
		    sh "docker login -u $USER -p $PASS"
		    sh 'docker push usama07/demo-app:ver-1.2'
		                     
            }
        }
}
        stage("deploy") {
            steps {
		    
                    echo "deploying the application version"
                    
            
        }   
    }
  }
}
