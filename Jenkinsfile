pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/BerkyTech/Michaeldjango.git']]
                ])
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                python3 -m venv venv || true
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirement.txt
                '''
            }
        }

        stage('Migrate DB') {
            steps {
                sh '''
                . venv/bin/activate
                python manage.py migrate
                '''
            }
        }

        stage('Run Server') {
            steps {
                sh '''
                pkill -f "manage.py runserver 0.0.0.0:8000" || true
                nohup venv/bin/python manage.py runserver 0.0.0.0:8000 > django.log 2>&1 &
                '''
            }
        }
    }
}