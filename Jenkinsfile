pipeline {
    agent any
    tools {
        maven 'my_mvn'
    }
    stages {
        stage("Checkout") {   
            steps {               	 
                git branch: 'main', url: 'https://github.com/manikcloud/manik-calculator.git'        	 
            
                // git branch: '8.1-addressbook', url: 'https://github.com/manikcloud/Jenkins-cicd.git'        	 
            }    
        }
        stage('Maven Clean') {
            steps {
                sh "mvn clean"  	 
            }
        }
        stage('Maven Build') {
            steps {
                sh "mvn compile"  	 
            }
        }
        stage("Unit Test") {          	 
            steps {  	 
                sh "mvn test"          	 
            }
        }
        stage("Unit validate") {          	 
            steps {  	 
                sh "mvn validate"          	 
            }
        }
        // stage("Unit Verify") {          	 
        //     steps {  	 
        //         sh "mvn verify"          	 
        //     }
        // }
        stage('Publish Unit Test Report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }               
        // stage("SonarQube Analysis") {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'sonarqube', passwordVariable: 'password', usernameVariable: 'username')]) {
        //             withSonarQubeEnv('sonarqube-server') {
        //                 sh "mvn verify sonar:sonar -Dsonar.host.url=http://${SERVER_IP}:9000 -Dsonar.login=${username} -Dsonar.password=${password}"
        //             }
        //         }
        //     } 
        // }
        stage("Maven Package") {
            steps {
                sh "mvn package" 
            }
        }
        // stage("Checkstyle") {
        //     steps {
        //         sh "mvn checkstyle:checkstyle"
        //         checkstyle canComputeNew: true, defaultEncoding: '', healthy: '', pattern: 'target/checkstyle-result.xml', unHealthy: ''
        //     }
        // }

        stage('Build and Static Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
                recordIssues tools: [checkStyle(pattern: '**/checkstyle-result.xml')]
            }
        }
        stage('SpotBugs Analysis') {
            steps {
                sh 'mvn spotbugs:check'
                recordIssues tools: [spotBugs(pattern: '**/target/spotbugsXml.xml')]
            }
        }




        // stage("FindBugs Analysis") {
        //     steps {
        //         sh "mvn findbugs:findbugs"
        //         findbugs pattern: 'target/findbugsXml.xml'
        //     }
        // }

        // stage("Deploy On Server") {          	 
        //     steps {  	 
        //         deploy adapters: [tomcat9(credentialsId: 'tomcat-9', path: '', url: "http://${SERVER_IP}:8090")], contextPath: '/manik-calculator', war: '**/target/*.war'         	 
        //     }
        // }  	
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo "Build Success"
            // emailext (
            //     to: 'varunmanik1@gmail.com',
            //     subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            //     body: "The job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' completed successfully."
            // )
        }
        failure {
            echo "Build Failed"
            // emailext (
            //     to: 'varunmanik1@gmail.com',
            //     subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            //     body: "The job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed."
            // )
        } 
    }
}
