// THis is pre-build Section //
pipeline {
    agent {                               // pre-build
        node{
            label 'AGENT-1'
        }
    }
         environment {
        COURSE = "Jenkins"
        ACC_ID = "807208358920"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 10, unit: 'MINUTES')     //If the pipeline runs longer than 10 minutes, Jenkins will automatically abort it.
        disableConcurrentBuilds()       // Only ONE build can run at a time for this pipeline.
    }

    // this is build section \\
    stages {
        stage('Read Version') {
            steps {
                script{
                  def packageJSON = readJSON file: 'package.json'   // versioning
                  appVersion = packageJSON.version
                  echo "app version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies ') {                    // build
            steps {
                script{
                    sh """
                        npm install
                    """    
                }
                
            }
        }
        stage ('Build Image'){
            steps{
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker images
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                    }
                }
                
            }
        }
        stage('Deploy') {
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
            // }
            when {
                expression { "$params.Deploy==true" }
            }
            steps {
                script{
                    sh """
                        echo "Deploying"
                    """
                }
            }
        }
    }
    // This is Post Section \\
    post {
        always {
            echo 'I will always say Hello again'  // this step is for it will execute even pipeline failed case also.
            cleanWs()                             //clean the workspace after the build finishes whther it may be failes / sucess

        }
        success {
            echo 'I will run if success'            //  This is post build section 
        }
        failure {
            echo 'I will run if failure'   
        }
        aborted {
            echo 'pipeline is aborted'
        }
    }
}
