#!/usr/bin/env groovy

library identifier: 'js-jenkins-shared-library@main', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://github.com/farru1998/js-shared-library-main.git',
         credentialsId: 'github-credentials'
        ]
)

def gv

pipeline {
    agent any
    stages {
        stage('init') {
            steps {
                script {
                    gv = load 'script.groovy'
                }
            }
        }
        stage('check branch') {
            steps {
                script {
                    branch()
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    buildImage 'mfarhan1998/simpleapp:1.0'
                    // dockerLogin()
                    // dockerPush 'armughanahmed/shared-lib-app:sla-3.0'
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    gv.deployApp() 
			sshPublisher(
					publishers: 
					     [
						     sshPublisherDesc
						     (
							configName: 'server2', 
							transfers: 
							[
								sshTransfer(
									cleanRemote: false, 
									excludes: '', 
									execCommand: 'kubectl set image deployment/simpleapp-deployment k8s_POD_simpleapp-deployment-5467bff66-59pdn_default_85377c89-286a-43d5-afd5-e3848772ede5_0=mfarhan1998/simpleapp:1.0', 
									execTimeout: 120000, 
									flatten: false, 
									makeEmptyDirs: false, 
									noDefaultExcludes: false, 
									patternSeparator: '[, ]+', 
									remoteDirectory: '', 
									remoteDirectorySDF: false, 
									removePrefix: '', 
									sourceFiles: ''
								)
						], 
						usePromotionTimestamp: false, 
						useWorkspaceInPromotion: false, 
						verbose: false
						     )
					     ]
				    )
                }
            }
        }
    }
}
