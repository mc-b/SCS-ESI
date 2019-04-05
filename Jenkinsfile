pipeline {
    agent none
    stages {
        stage('Build') {
		    agent {
		        docker {
		            image 'maven:3-alpine'
		            args '-v /root/.m2:/root/.m2'
			    }
		    }         
            steps {
                sh 'cd scs-demo-esi-order/ && mvn -B clean package site'
                archiveArtifacts 'scs-demo-esi-order/target/*.jar'
                archiveArtifacts 'scs-demo-esi-order/target/site/**'
                stash includes: 'scs-demo-esi-order/target/*.jar', name: 'jar'
            }
        }
        stage('Build Images') { 
        	agent any
            steps {
            		unstash 'jar'
            		sh 'ls -l scs-demo-esi-order/target/'
            		sh 'cd docker/varnish      && /usr/bin/docker build -t misegr/scsesi_varnish .'
            		sh 'cd scs-demo-esi-common && /usr/bin/docker build -t misegr/scsesi_common .'
            		sh 'cd scs-demo-esi-order  && /usr/bin/docker build -t misegr/scsesi_order .'
            }
        }
    }
}
