#!groovy
pipeline {
    agent any
    parameters {
        string(
                name: "branch",
                defaultValue: "master",
                description: "Бренч откуда клонить"
        )
    }
    stages {
        stage("init not Master") {
            when {
                not { expression { return "$branch" == "master" }  }
            }
            steps {
                sh 'echo "not Master"'
            }
        }
        stage("otus") {
            input {
                message "Should we continue?"
                ok "Yep"
                parameters {
                    string(
                            name: "admin name",
                            defaultValue: "root",
                            description: ""
                    )
                }
            }
            steps {
                sh 'echo "Otus Jenkins"'
            }
        }
    }
}