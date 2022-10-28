pipeline {
    agent any
    tools {
	maven 'maven'
}	
    stages {
	   
	stage("ïncrement version") {
		steps {
				echo 'incrementing app version'
				sh 'mvn build-helper:parse-version versions:set \			
					-DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \			
					versions:commit' 
				def match = readFile('pom.xml') =~ '<version>(.+)</version>'   
				env.IMAGE_NAME = match[0][1]      
	    
		}
	}
	    
	    
	    
        stage("build jar") {
            steps {
		    
                echo "building applic version"
		sh 'mvn clean package'
		    
	    }                        
          }
    	stage("build image") {
            steps {
		    
                echo "building docker image"
		withCredentials([usernamePassword(credentialsId: 'docker-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
			  
		    sh 'docker build -t usama07/demo-app:${IMAGE_NAME} .'
		    sh "docker login -u $USER -p $PASS"
		    sh 'docker push usama07/demo-app:${IMAGE_NAME}'
		                     
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
