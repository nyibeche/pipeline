node {
   def commit_id
   stage('Readiness') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('testrun') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
       sh 'npm ls -g'
       sh 'npm update' 
     }
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("belushi/dockernodejsimage:${commit_id}", '.').push()
     }
   }
}
