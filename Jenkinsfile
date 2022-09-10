node() {

    try {

       stage('Checkout'){

          checkout scm
       }

       stage('NPM Install'){
         sh 'node -v'
         dir('users-service') {
   nodejs('NodeJS'){
   bat "npm install"
   }
			
		 }
       }
       stage('NPM Unit Test'){
		 dir('users-service') {
			bat "npm test"
		 }
       }

       stage('Container Build'){
		 sh "docker-compose build --force"
       }

	   stage('Container Deploy'){
		 sh "docker-compose up -d"
       }
	   
	   stage('Node Integration Testing'){
		 dir('integration-test') {
		    sleep 20
		    bat "npm install && npm start"
		 }
       }
	   
       stage('Cleanup'){
		 bat 'npm cache clean'
		 sh 'docker-compose down'

         mail body: 'project build successful',
                     from: 'jenkins@buildserver.com',
                     replyTo: 'pavanshivanand@gmail.com',
                     subject: 'project build successful',
                     to: 'pavanshivanand@gmail.com'
       }



    }
    catch (err) {
        currentBuild.result = "FAILURE"
            mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'jenkins@buildserver.com',
            replyTo: 'pavanshivanand@gmail.com',
            subject: 'project build failed',
            to: 'pavanshivanand@gmail.com'
	sh "docker-compose down"
        throw err
    }

}
