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
	    
stage('git version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // git config here for the first time run
                        sh 'git config --global user.email "iam_usama017@outlook.com"'
                        sh 'git config --global user.name "Usama_017"'

                        sh "git remote set-url origin https://${USER}:${PASS}@https://github.com/Usama017/test.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:MASTER'
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
}
