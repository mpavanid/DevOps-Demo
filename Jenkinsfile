pipeline {
    agent {
        label "master"
    }
    tools {
        maven 'Maven3'

    }
    stages {
        

        stage ('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/BR_EppalapellyR']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/devops81/DevOps-Demo.git']]])
                
            }
        }
        
        stage ('Build the project') {
            steps {
                dir("/var/lib/jenkins/workspace/BR_ER_Pipeline/examples/feed-combiner-java8-webapp") {
             sh 'mvn clean install'
                }
                
            }
        }
          stage ('Generate JUNIT REPORT') {
             steps {
                  parallel ( 
                      'Archeiving the reports': 
            {
                junit 'examples/feed-combiner-java8-webapp/target/surefire-reports/*.xml'
                
            },
                      'Sending out the JUNIT report' :
                      {                  
                         emailext body: 'Junits reporting getting archived', subject: 'junit update', to: 'devops81@gmail.com'
                     }
                     )
            } 
        }
        stage ('Deploy the application') {
            steps {
               
                sh 'cp  -rf  /var/lib/jenkins/workspace/BR_ER_Pipeline/examples/feed-combiner-java8-webapp/target/devops_RE.war /home/jarfile'
                
            }
        }
        stage ('Send out email Notification') {
            agent {
                label "master"
            }
            steps {
                emailext body: '$DEFAULT_CONTENT', subject: '$DEFAULT_SUBJECT', to: 'devops81@gmail.com'

                
            }
        }
        }
       
        
}
