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
        stage("init not Master") {
            when {
                not { equals { expected: "master", actual: "$branch" } }
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