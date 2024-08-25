pipeline {
    agent any

    environment {
        NODE_VERSION = '18.16.0'
        YARN_VERSION = '3.4.1' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the repository...'
                git branch: 'main', url: 'https://github.com/vincdac/AquilaCMS.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'curl -sL https://deb.nodesource.com/setup_${NODE_VERSION} | bash -'
                sh 'apt-get install -y nodejs'
                sh 'npm install -g yarn@${YARN_VERSION}'
                sh 'yarn install'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm start'
            }
        }
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying the application...'
                // Define your deployment script or commands here
                sh '''
                # Example deployment steps
                docker build -t aquila-cms .
                docker run -d -p 3000:3000 aquila-cms
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
        success {
            echo 'Build was successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
