pipeline {
    agent any
    triggers { 
        pollSCM('*/1 * * * *') 
    }
    stages {
        stage('build-install-deps') {
            when {
                not {
                    changeset "README.md"
                }
            }
            steps { 
                script{
                    build()
                }
            }
        }
        stage('deploy-dev') {
            steps {
                script{
                    deploy("DEV", 3000)
                }
            }
        }
        stage('test-dev') {
            steps {
                script{
                    test("DEV")
                }
            }
        }
        stage('deploy-stg') {
            steps {
                script{
                    deploy("STG", 4000)
                }
            }
        }
        stage('test-stg') {
            steps {
                script{
                    test("STG")
                }
            }
        }
        stage('deploy-prd') {
            steps {
                script{
                    deploy("PRD", 5001)
                }
            }
        }
        stage('test-prd') {
            steps {
                script{
                    test("PRD")
                }
            }
        }
    }
}

def build(){
    echo "Installing all necessary node dependencies.." 
    sh "npm install"
    echo "Dependecies successfully installed.." 
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} environment has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/sample-book-app-2026.git'
    sh "npm install"
    sh "ls"
    sh "node_modules/.bin/pm2 delete \"books-${environment}\" || exit 0"
    sh "node_modules/.bin/pm2 start -n \"books-${environment}\" index.js -- ${port}"
    // sh "node_modules/.bin/pm2 reload -n \"books-${environment}\" index.js -- ${port}" //using 1 command to relaod service
    // sh "pm2 start -n "books-${environment}" index.js -- ${port}"
    // sh "pm2 start -n \"books-${environment}\" index.js -- ${port}"
    // bat "node_modules\\.bin\\pm2 start -n \"books-${environment}\" index.js -- -- ${port}"
    echo "Deployment to ${environment} environment finished.."
}

def test(String environment){
    echo "Testing Sample Book Application service has started on ${environment} environment.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/RTU-sample-API-automation-2026.git'
    sh "npm install"
    sh "npm run books BOOKS_${environment}"
    echo "Testing Sample Book Application service finished.."
}
