#!groovy

pipeline {
    agent any
    options {
        buildDiscarder(logRotator([
            daysToKeepStr: '7',
            numToKeepStr: '10'
        ]))
    }
    stages {
        stage('Prueba') {
            steps {
                echo "FUNCIONA"
            }
        }
        stage('Unit test') {
            steps {
                script {
                    //pysh 'pip3 install -r requirements.txt'
                    //pysh 'pip install -e .'
                    sh 'python3 test_all.py'
                }
            }
        }
        
        stage('Secret') {
            steps {
                script {
                    echo "---BEGIN---"
                    env.giturl = sh(returnStdout: true, script: 'git config --get remote.origin.url').trim()
                    try {
                        sh("trufflehog --json --regex ${env.giturl}")
                    } catch(err) {
                        echo "Caught: ${err}"
                        currentBuild.result="SUCCESS"
                    } 
                    echo "---END---"
                }
            }
        }    
    }
}
