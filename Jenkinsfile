pipeline {
    agent { label 'docker-agent-1' }

    environment {
        GIT_BRANCH_NAME = 'new'
        GIT_REPO_URL = 'https://github.com/Zvnazar00/flask_api_devops.git'
        RUNNER_CONTAINER = 'test_runner'
    }

    stages {

        // 1. Checkout from SCM
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: "${GIT_REPO_URL}"
            }
        }

        // 2. Run Docker Compose Tests
        stage('Docker Compose Build') {
            steps {
                script {
                    sh """
                        echo "Running docker compose tests for flask API"
                        echo "--------------------------------------------"
                        docker compose stop || true
                        docker compose build --no-cache
                        docker compose up --abort-on-container-exit --exit-code-from ${RUNNER_CONTAINER}
                        echo "Tests finished"
                        docker compose stop || true
                    """
                }
            }
        }

        // 3. Push to GitHub (new branch)
        stage('Push to GitHub Branch') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '756c1b00-6caa-4e0f-98dc-8dd18d0b03dc', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh """
                            git config user.name "${GIT_USERNAME}"
                            git config user.email "${GIT_USERNAME}@users.noreply.github.com"

                            git checkout -b ${GIT_BRANCH_NAME} || git checkout ${GIT_BRANCH_NAME}
                            git add .
                            git commit -m "CI: Push after successful test run" || echo "Nothing to commit"
                            git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Zvnazar00/flask_api_devops.git
                            git push origin ${GIT_BRANCH_NAME} --force
                        """
                    }
                }
            }
        }

    }
}
