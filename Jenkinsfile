pipeline {
    agent any
    environment {
        REPO1_URL = 'git@github.com:awaisatiq/jenkins-seed.git'
        REPO2_URL = 'git@github.com:awaisatiq/jenkins-peer.git'
        TEMP_DIR = 'temp_repo'
    }
    parameters {
        string(name: 'MESSAGE', defaultValue: 'RELEASE 07.10.2024', description: 'Commit message')
        string(name: 'FROM_BRANCH', defaultValue: 'main', description: 'From branch (repo 1)')
        string(name: 'TO_BRANCH', defaultValue: 'main', description: 'To branch (repo 2)')
        string(name: 'FILE_PATHS', defaultValue: 'src/', description: 'Comma-separated list of files/folders')
    }
    stages {
        stage('Clone repo 1 and Checkout From Branch') {
            steps {
                // Clone repo1
                sh 'rm -rf $TEMP_DIR'
                sh 'git clone -b ${FROM_BRANCH} ${REPO1_URL} $TEMP_DIR/repo1'
            }
        }
        stage('Clone repo 2 and Checkout To Branch') {
            steps {
                // Clone repo2
                sh 'git clone -b ${TO_BRANCH} ${REPO2_URL} $TEMP_DIR/repo2'
            }
        }
        stage('Copy Files and Folders') {
            steps {
                script {
                    def paths = params.FILE_PATHS.split(',')
                    for (path in paths) {
                        sh "cp -r $TEMP_DIR/repo1/${path.trim()} $TEMP_DIR/repo2/"
                    }
                }
            }
        }
        stage('Commit and Push Changes to Repo 2') {
            steps {
                dir("$TEMP_DIR/repo2") {
                    sh 'git add .'
                    sh "git commit -m \"${MESSAGE}\""
                    sh 'git push origin ${TO_BRANCH}'
                }
            }
        }
    }
    post {
        always {
            // Cleanup workspace
            sh 'rm -rf $TEMP_DIR'
        }
    }
}
