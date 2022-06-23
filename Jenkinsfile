#!groovy
pipeline {
    agent any
    parameters {
        string(
            name: "branch",
            defaultValue: "master",
            description: "Бренч по умолчанию"
        )
    }
    stages {
        stage("init") {
            steps {
                sh "echo 'init'"
            }
        }
        stage("Not For Master") {
            when {
                not { expression { return "$branch" == "master" } }
            }
            steps {
                sh "echo 'not master'"
            }
        }
    }
}
