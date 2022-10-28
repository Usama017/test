pipeline {
    agent any
    tools {
	maven 'maven'
}	
    stages {   
	stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
  
	    
        stage('build jar') {
            steps {
		script {  
                  echo "building applicat version"
		  sh 'mvn clean package'
		    
         }                        
      }
   }
    	stage('build image') {
            steps {
		 script {    
                    echo "building docker image"
		    withCredentials([usernamePassword(credentialsId: 'docker-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
			  
		    sh 'docker build -t usama07/demo-app:${IMAGE_NAME} .'
		    sh "docker login -u $USER -p $PASS"
		    sh 'docker push usama07/demo-app:${IMAGE_NAME}'
		                     
            }
        }
   }
}
	    
	stage('git version update') {
            steps {
                script {
                        
			withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        
			
			
			sh "git remote set-url origin https://${USER}:${PASS}@github.com/Usama017/test.git"
			sh 'git init'
                        sh 'git add .'
                        sh "git commit -m 'ci: version bump'"
                        sh 'git push origin HEAD:master'
		  }
	    }
	}
}
        stage('deploy') {
            steps {
		    script {
                    echo "deploying the application version"
                    
            
         }   
       }
     }
   }
  }
