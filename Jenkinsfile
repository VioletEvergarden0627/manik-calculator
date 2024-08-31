pipeline {
    agent any
    tools {
        maven 'my_mvn'
    }
    stages {
        stage("Checkout") {   
            steps {               	 
                git branch: 'main', url: 'https://github.com/VioletEvergarden0627/manik-calculator.git'        	 
            }    
        }
        stage('Maven Build and Test') {
            steps {
                sh "mvn clean compile test"
            }
        }
        stage("Unit validate") {          	 
            steps {  	 
                sh "mvn validate"          	 
            }
        }
        stage('Publish Unit Test Report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }               
        stage("Maven Package") {
            steps {
                sh "mvn package" 
            }
        }
        stage('Static Code Analysis') {
            parallel {
                stage('Checkstyle') {
                    steps {
                        sh 'mvn checkstyle:checkstyle'
                        recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])
                    }
                }
                stage('FindBugs') {
                    steps {
                        sh 'mvn findbugs:findbugs'
                        recordIssues(tools: [findBugs(pattern: '**/findbugsXml.xml')])
                    }
                }
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo 'Build succeeded!!!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
