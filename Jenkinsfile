pipeline {
    agent any
    parameters {
        string(name: 'NAME', defaultValue: 'World', description: '')
    }
    stages {
        stage('Hello') {
            steps {
                echo "Hello ${params.NAME}!"
            }
        }
    }
}
