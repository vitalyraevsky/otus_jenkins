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