pipeline {
agent {
  label "master"
}
tools {
  maven 'Maven3'
}
stage('Initialize') {
  steps {
    sh '''
          echo "PATH = $PATH"
          echo "M2_HOME = $M2_HOME"
       '''
  }
}
stage('checkout') {
  steps {
    checkout([$class: 'GitSCM', branches: [[name: '*/BR_Manoj']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'bbcb3036a7c3b14ae76454d2ebf7ae174e06c0dc', url: 'https://github.com/devops81/DevOps-Demo.git']]])
  }
}
stage('Build the project') {
  steps {
    // One or more steps need to be included within the steps block.
  }
}  stage ('Build the project') {
            steps {
                dir("/var/lib/jenkins/workspace/PipelineProject/examples/feed-combiner-java8-webapp") {
             sh 'mvn clean install'
                }
                
            }
        }
		     stage ('Deploy the application') {
            steps {
               
                sh 'cp  -rf  /var/lib/jenkins/workspace/PipelineProject/examples/feed-combiner-java8-webapp/target/devops.war/home/jarfile'
                
            }
        }
        stage ('Send out email Notification') {
            agent {
                label "master"
            }
            steps {
                emailext body: '$DEFAULT_CONTENT', subject: '$DEFAULT_SUBJECT', to: 'devops81@gmail.com,manoj.broadridge.com'

                
            }
        }
	
}
