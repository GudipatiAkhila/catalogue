// THis is pre-build Section //
pipeline {
    agent {                               // pre-build
        node{
            label 'AGENT-1'
        }
    }
         environment {
        COURSE = "Jenkins"
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
        stage('Test') {                    // build
            steps {
                sh """
                   echo "Testing"

                """
                
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
