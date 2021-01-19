node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("angadi77/signing")
    }

    

     stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
	    
     }
	stage('analyze') {
             def imageLine = "https://registry.hub.docker.com/angadi77/signing:latest"
            writeFile file: 'anchore_images', text: imageLine
            anchore name: 'anchore_images', bailOnFail: false, bailOnPluginFail: false, engineRetries: '1200'
        }
	
	
	
}
