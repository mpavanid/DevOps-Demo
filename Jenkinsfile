pipeline {
    agent {
        label "master"
    }
    tools {
        maven 'Maven3'

    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = $PATH"
                    echo "M2_HOME = $M2_HOME"
                '''
            }
        }

        stage ('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/BR_SANDEEP']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/devops81/DevOps-Demo.git']]])
                
            }
        }
        
        stage ('Build the project') {
            steps {
                dir("/var/lib/jenkins/workspace/SANDEEP_LAB/sandeep_pipeline/examples/feed-combiner-java8-webapp") {
             sh 'mvn clean install'
                }
                
            }
        }
          
        stage ('Deploy the application') {
            steps {
               
                sh 'cp  -rf  /var/lib/jenkins/workspace/SANDEEP_LAB/sandeep_pipeline/examples/feed-combiner-java8-webapp/target/sandeep_devops_1.war /home/jarfile'
                
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
        
        stage ('Parallel jobs execution') {
            steps {
                parallel firstBranch: {
                    build 'SANDEEP_LAB/sandeep_pipeline_1'
                    }, secondBranch: {
                    build 'SANDEEP_LAB/sandeep_pipeline_2'
                    },
                 failFast: true
            }
        }
              
}
}
