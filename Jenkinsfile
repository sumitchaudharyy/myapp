pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Create Environment') {
            steps {
                sh '''
                    python3 -m venv flask-env
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    flask-env/bin/pip install --upgrade pip
                    flask-env/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test Flask App') {
            steps {
                sh '''
                    flask-env/bin/python -m py_compile app.py
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                    pkill -f "flask-env/bin/python app.py" || true
                    nohup flask-env/bin/python app.py > flask.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo 'Flask pipeline completed successfully!'
        }

        failure {
            echo 'Flask pipeline failed!'
        }
    }
}
