node {
   def commit_id
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('Compile & Build') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }
      stage('Test execution'){
         nodejs(nodeJSInstallationName: 'nodejs'){  
            sh 'npm test'
      }
      
   }
   stage('Docker build/push') {
       docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("manee2k6/docker-nodejs:app-${commit_id}", '.').push()
     }
   }
}
