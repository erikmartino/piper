pipeline {
    agent none
    stages {
        stage("A") {
            agent any
            steps {
                echo 'A'
                sh 'date >a.out'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'a.out'
                }
            }
        }
        stage("B") {
            agent any
            steps {
                sh 'date >b.out'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'b.out'
                }
            }
        }
    }
}
