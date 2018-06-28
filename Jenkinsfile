node {
    docker.withRegistry('https://hub.docker.com/','ritu25','docker@123','org.jenkinsci.plugins.workflow.cps.CpsClosure2@3939ff9a') {
    	def mvnHome
    	
    	stage('checkout'){
        
       
        git url: 'https://github.com/ritu2593/Kubernetes.git'
    
        sh "git rev-parse HEAD > .git/commit-id"
        def commit_id = readFile('.git/commit-id').trim()
        println commit_id
        }
    
    dir('${artifactId}'){
        stage('build'){
	        sh "mvn -Dmaven.test.failure.ignore clean install"
	        sh 'docker login --username <userName> --password <password>'
	        sh ("docker build -t ${artifactId} .")
	        sh ("docker tag  ${artifactId} ritu25/test:${artifactId}")
	        sh ("docker push <repo>/test:${artifactId}")
    	}
    	
    	stage('create docker image'){
		        sh 'docker login --username <user> --password <user>'
		        sh ("docker build -t address-rest-api .")
		        sh ("docker tag  address-rest-api <repo>/test:address-rest-api")
    	}
    	
    	stage('push docker image'){
			sh ("docker push ritu25/test:${artifactId}")
    	}
    	
    	stage('create deployment'){
    	    sh 'kubectl delete deployments ${artifactId}api || true' 
	    sh 'kubectl create -f deployment.yaml --validate=false'
    	}
    	
    	stage('create service'){
	    sh 'kubectl delete services ${artifactId}apiservice || true'
	    sh 'kubectl create -f services.yaml --validate=false'
    	}
    }
    }
}
