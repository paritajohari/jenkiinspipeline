pipeline {
    agent { docker 'maven:3-alpine' } 
    options { timeout(time: 1, unit: 'HOURS') }
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                echo env.BUILD_ID
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'mvn --version'
            }
        }
        stage('Test') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo 'Testing..'
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                echo 'Deploying....'
                echo "Hello ${params.PERSON}"
            }
        }        
    }
    post {
        always {
            //generate cucumber reports
            cucumber 'cucumber-json-report.json'
        }
    }
}
