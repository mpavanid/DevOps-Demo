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
                checkout([$class: 'GitSCM', branches: [[name: '*/BR_SUPRABHAT']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/devops81/DevOps-Demo.git']]])
                
            }
        }
        
        stage ('Build the project') {
            steps {
                dir("/var/lib/jenkins/workspace/test_java_pipe/examples/feed-combiner-java8-webapp") {
             sh 'mvn clean install'
                   
                }
                
            }
        }
        
        state (' Parallel Deploy ') {
            steps {
                     parallel firstBranch: {
		                    build 'test_java_1'
                                }, secondBranch: {
                            build 'test_java_2'
                        },
                    failFast: true
            }
        }
        stage ('Deploy the application') {
            steps {
               
                sh 'cp  -rf  /var/lib/jenkins/workspace/test_java_pipe/examples/feed-combiner-java8-webapp/src/main/resources/logback.xml /home/jarfile'
                
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

