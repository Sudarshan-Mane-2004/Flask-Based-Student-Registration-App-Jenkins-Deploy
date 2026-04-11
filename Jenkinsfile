pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
    steps {
        sh '''
        sudo apt update
        sudo apt install -y python3-venv || true

        python3 -m venv venv
        . venv/bin/activate
        pip install -r requirements.txt
        '''
    }
}

        stage('Stop Old App') {
            steps {
                sh '''
                pkill -f app.py || true
                pkill gunicorn || true
                '''
            }
        }

        stage('Run Application') {
    steps {
        sh '''
        . venv/bin/activate
        pkill -f app.py || true
        nohup python3 app.py > app.log 2>&1 &
        sleep 5
        ps -ef | grep app.py
        '''
    }
}

        stage('Verify App') {
            steps {
                sh '''
                sleep 5
                curl -I http://localhost:5000 || true
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Success"
        }
        failure {
            echo "❌ CI/CD Failed"
        }
    }
}