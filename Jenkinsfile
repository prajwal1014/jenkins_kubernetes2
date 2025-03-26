pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/prajwal1014/jenkins_kubernetes2.git"
        BRANCH = "main"
    }

    stages {
        stage('Checkout Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Retrieve admin.conf') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG
                    kubectl config view --raw > admin.conf
                    '''
                }
            }
        }

        stage('Commit and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh '''
                    git config --global user.email "jenkins@example.com"
                    git config --global user.name "Jenkins Automation"

                    git add admin.conf
                    git commit -m "Updated admin.conf from Jenkins"
                    git push https://$GIT_USER:$GIT_PASS@github.com/prajwal1014/jenkins_kubernetes2.git main
                    '''
                }
            }
        }
    }
}
