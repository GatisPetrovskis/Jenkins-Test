def installPythonDeps() {
    bat 'echo Installing all required Python dependencies...'
    bat 'if exist python-greetings rmdir /s /q python-greetings'
    bat 'git clone https://github.com/mtararujs/python-greetings python-greetings'
    bat 'cd python-greetings && dir'
    bat 'cd python-greetings && python -m venv venv'
    bat 'cd python-greetings && venv\\Scripts\\python -m pip install -r requirements.txt'
}

def deployService(envName, port) {
    bat "echo Deploying to ${envName}..."
    bat 'if exist python-greetings rmdir /s /q python-greetings'
    bat 'git clone https://github.com/mtararujs/python-greetings python-greetings'
    bat 'cd python-greetings && python -m venv venv'
    bat 'cd python-greetings && venv\\Scripts\\python -m pip install -r requirements.txt'
    bat "cd python-greetings && C:\\Users\\Gatis\\AppData\\Roaming\\npm\\pm2.cmd delete greetings-app-${envName} || exit /b 0"
    bat "cd python-greetings && set PORT=${port} && C:\\Users\\Gatis\\AppData\\Roaming\\npm\\pm2.cmd start app.py --name greetings-app-${envName} --interpreter \"%WORKSPACE%\\python-greetings\\venv\\Scripts\\python.exe\""
}

def testService(envName) {
    bat "echo Testing on ${envName}..."
    bat 'if exist course-js-api-framework rmdir /s /q course-js-api-framework'
    bat 'git clone https://github.com/mtararujs/course-js-api-framework course-js-api-framework'
    bat 'cd course-js-api-framework && npm install'
    bat "cd course-js-api-framework && npm run greetings -- greetings_${envName}"
}

pipeline {
    agent any

    stages {
        stage('install-pip-deps') {
            steps {
                script { installPythonDeps() }
            }
        }

        stage('deploy-to-dev') {
            steps {
                script { deployService('dev', '7001') }
            }
        }

        stage('tests-on-dev') {
            steps {
                script { testService('dev') }
            }
        }

        stage('deploy-to-stg') {
            steps {
                script { deployService('stg', '7002') }
            }
        }

        stage('tests-on-stg') {
            steps {
                script { testService('stg') }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                script { deployService('preprod', '7003') }
            }
        }

        stage('tests-on-preprod') {
            steps {
                script { testService('preprod') }
            }
        }

        stage('deploy-to-prod') {
            steps {
                script { deployService('prod', '7004') }
            }
        }

        stage('tests-on-prod') {
            steps {
                script { testService('prod') }
            }
        }
    }
}
