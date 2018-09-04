
/**
 * Tests that all the previous releases are able to upgrade to the last build
 */
def installAirgapCredentials() {
    withCredentials([[$class          : 'UsernamePasswordMultiBinding', credentialsId: '92935adf-cb3d-4f9a-847e-9011d6e0d9af',
                      usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
        sh 'bash ops/airgap/jenkins/install_artifactory_credentials.sh $USERNAME $PASSWORD'
    }
    withCredentials([[$class          : 'UsernamePasswordMultiBinding', credentialsId: 'airgap-download-apikey',
                      usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
        sh 'bash ops/airgap/jenkins/install_download_apikey.sh $USERNAME $PASSWORD'
    }
    withCredentials([file(credentialsId: 'martinosvc', variable: 'KEY_FILE')]) {
        sh 'bash ops/airgap/jenkins/install_gcloud_credentials.sh $KEY_FILE'
    }
}

def upgradeReleases(snapshot_name) {
    copyArtifacts(projectName: "Airgap/axon-installer/airgap-next",
            fingerprintArtifacts: true,
            target: '.',
            selector: lastSuccessful()
    )
    sh "mkdir -p ops/out/logs/;echo >>ops/out/logs/README.txt Upgrading snapshot axon-2018-05-29 " + 'to $(ls -l ops/out/*.sh)'
    sh "echo gcloud compute snapshots describe axon-2018-05-29 && echo " +
            "./ops/airgap/bin/airgapctl run --instance axon-2018-05-29 " +
            "--snapshot axon-2018-05-29 " +
            '--installer $(pwd)/ops/out/axon-installer-*.sh ' +
            '--run-tests ' +
            target_height + ' ' +
            "--logs " +
            "--save-snapshot-on-failure-as axon-2018-05-29-failed " +
            '--delete-instance '
}


pipeline {
    agent {
        label 'airgap'
    }

    stages {
        stage('Upgrade all') {
            parallel {
                stage('A') {
                    agent {
                        dockerfile {
                            filename 'Dockerfile'
                            label 'airgap'
                        }
                    }

                    steps {
                        script {
                            installAirgapCredentials()
                            upgradeReleases("A")
                        }
                    }
                }
                stage('B') {
                    agent {
                        dockerfile {
                            filename 'Dockerfile'
                            label 'airgap'
                        }
                    }
                    steps {
                        script {
                            installAirgapCredentials()
                            upgradeReleases("B")
                        }
                    }
                }
            }
        }
    }
}
