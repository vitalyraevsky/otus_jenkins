#!groovy
pipeline {
    agent any
    parameters {
        string(
                name: "branch",
                defaultValue: "master",
                description: "Бренч от куда клонить"
        )
    }
    stages {
        stage("otus") {
            steps {
                sh 'echo "Otus Jenkins"'
            }
        }
    }
}