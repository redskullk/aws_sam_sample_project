pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/redskullk/aws_sam_sample_project.git']]])
            }
        }
	stage('validation') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws_credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
		    sh 'sam --version'
                }     
            }
        }    
        stage('build') {
            steps {
		withCredentials([usernamePassword(credentialsId: 'aws_credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    //sh 'sam package --template-file template.yaml --output-template-file sam.yaml --s3-bucket samawstestbucket'
		    nodejs('node'){
			sh 'sam build'
		    }	
                }    
	    }
        }
         stage('deploy') {
            steps {
		withCredentials([usernamePassword(credentialsId: 'aws_credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                     sh 'sam deploy --template-file sam.yaml --region ap-south-1 --stack-name sam-rest-api --capabilities CAPABILITY_IAM'
                }    
            }
        }

    }
    
}
