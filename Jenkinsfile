pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/prajwal1014/jenkins_kubernetes2.git'
        GIT_CREDENTIALS = 'github-token'
        K8S_CREDENTIALS = 'k8s-admin-conf'
        ADMIN_CONF_FILE = 'admin.conf'
    }
    stages {
        stage('Checkout Repository') {
    steps {
        git branch: 'main', 
            url: "https://github.com/prajwal1014/jenkins_kubernetes2.git", 
            credentialsId: "github-token"
    }
}

        stage('Retrieve admin.conf') {
            steps {
                withCredentials([file(credentialsId: "${K8S_CREDENTIALS}", variable: "ADMIN_CONF")]) {
                    sh "cp ${ADMIN_CONF} ${ADMIN_CONF_FILE}"
                }
            }
        }
        stage('Commit and Push') {
    steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
            sh '''
            git config --global user.email "jenkins@example.com"
            git config --global user.name "Jenkins Automation"
            git add admin.conf
            git commit -m "Updated admin.conf from Jenkins"
            git push https://$GITHUB_TOKEN@github.com/prajwal1014/jenkins_kubernetes2.git main
            '''
        }
    }
}

    }
}
