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
    triggers {
        pullSCM('H/15 * * * *')
    }
    stages {
        stage("otus") {
            steps {
                sh 'echo "Otus Jenkins"'
            }
        }
    }
}