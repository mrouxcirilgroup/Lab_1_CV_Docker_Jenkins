pipeline {
      agent any
      stages {
        // Clean image
        stage('Cleaning des images docker') {
            steps {
                script {
                    def exists = sh(script: "docker ps -a --format '{{.Names}}' | grep -w cv_mroux || true", returnStdout: true).trim()
                    if (exists) {
                        sh 'docker stop cv_mroux'
                        sh 'docker rm cv_mroux'
                    } else {
                        echo "Container cv_mroux does not exist, skipping stop/rm."
                    }
                }
            }
            post {
                success {
                    echo "====++++Docker images cleaning success++++===="
                }
                failure {
                    echo "====++++Docker cleaning failed++++===="
                }
            }
        }

        // Création image
        stage('Création de  image docker') {
            steps {
                sh 'docker build -t cv_mroux .'
            }
            post {
                success {
                    echo "====++++Docker image created with success++++===="
                }
                failure {
                    echo "====++++Docker image failed++++===="
                }
            }
        }

          // Run container
        stage('Lancer un container de cette image') {
            steps {
                sh 'docker run -d -p 8074:80 --name cv_mroux_cont cv_mroux'
            }
            post {
                success {
                    echo "====++++Container started with success++++===="
                }
                failure {
                    echo "====++++Failed to start Container++++===="
                }
            }
        }
      }
}