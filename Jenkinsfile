pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') 
    }

    environment {
        DOTNET_VERSION = '6.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup .NET') {
            steps {
                sh '''
                if ! dotnet --version >/dev/null 2>&1; then
                    wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
                    chmod +x dotnet-install.sh
                    ./dotnet-install.sh --version $DOTNET_VERSION
                    export PATH=$HOME/.dotnet:$PATH
                fi
                dotnet --version
                '''
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        success {
            echo "Build and tests succeeded!"
        }
        failure {
            echo "Build or tests failed."
        }
    }
}
