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
                archiveArtifacts artifacts: '*.out'
            }
        }
        stage("B") {
            agent any
            steps {
                sh 'date >b.out'
            }
            post {
                archiveArtifacts artifacts: '*.out'
            }
        }
    }
}
