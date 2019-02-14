pipeline {
    agent {
        label "master"
    }
    tools {
        maven 'Maven3'

    }
    stages {
        parallel{
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
                checkout([$class: 'GitSCM', branches: [[name: '*/br_prasad']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/devops81/DevOps-Demo.git']]])
                
            }
        }
        
        stage ('Build the project') {
            steps {
                dir("/var/lib/jenkins/workspace/PipelineProject/examples/feed-combiner-java8-webapp") {
             sh 'mvn clean install'
                }
                
            }
        }
          
        stage ('Deploy the application') {
            steps {
               
                sh 'cp  -rf  /var/lib/jenkins/workspace/PipelineProject/examples/feed-combiner-java8-webapp/target/devops.war /home/jarfile'
                
            }
        }
        stage ('Send out email Notification') {
            agent {
                label "master"
            }
            steps {
                emailext body: 'testjob1', subject: 'testjob1', to: 'Prasad.dugyala@broadridge.com'

                
            }
        }
        }
    }
        
        
    }              
