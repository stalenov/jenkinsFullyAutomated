pipeline {
    agent any

    parameters {
	string(name: 'tomcat_dev', defaultValue: '18.191.255.155', description: 'Staging server') 
	string(name: 'tomcat_prod', defaultValue: '18.222.144.46', description: 'Production server')
    }
	
    triggers {
	pollSCM('* * * * *')
    }

    stages {
	stage('Build'){
	    steps {
		sh 'mvn clean package'
  	    }
            post {
		success {
		    echo "Now archiving..."
		    archiveArtifacts artifacts: '**/target/*.war'		
		}
	    }
	}
	
	stage('Deployments'){
	    parallel{
		stage ('Deploy to staging') {
		    steps {
			 sh "scp -i /id_rsa **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
		    }
		}
		stage ('Deploy to production') {
                    steps { 
                         sh "scp -i /id_rsa **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
	    }
	}
    }
}


