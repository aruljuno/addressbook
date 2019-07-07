pipeline {
    
    agent { label 'slave' }
    stages {
	   
        stage ('Checkout') {
          steps {
            git 'https://github.com/aruljuno/addressbook.git'
          }
        }
        stage('Build') {
            
             steps {
               
               sh '/opt/apache-maven-3.6.1/bin/mvn clean package'
                //junit '**/target/surefire-reports/TEST-*.xml'
		archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
        stage('Docker image') {
        
            steps {
                
                sh 'docker build -t aruljuno/addressbook$(git rev-parse HEAD):latest .'
            }
    }
        stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'a8114fbf-7fd4-4138-82ce-674c49b68034', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push aruljuno/addressbook$(git rev-parse HEAD):latest'
        }
      }
    }
        stage('Docker Deploy') {
       
      steps {
          sh 'docker run -itd -P aruljuno/addressbook$(git rev-parse HEAD):latest'
        }
      }
    
    }
    
}
