pipeline {
    agent any
    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://54.160.176.39:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_68bec386c3ef0e2c4c94f18591340bc47501e575" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lmns'
                echo 'CODE QUALITY COMPLETED' 
            }
        }
        stage('Build LMS') {
            steps {
                echo 'Build LMS'
                sh 'cd webapp && npm install && npm run build'
                echo 'Build Completed'
            }
        }

        stage('Release LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    sh "zip webapp/lmns-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:134113114 --upload-file webapp/lmns-${packageJSONVersion}.zip http://54.160.176.39:8081/repository/lms/"
                }
            }
        }

        stage('Deploy LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    sh "curl -u admin:134113114 -X GET \'http://54.160.176.39:8081/repository/lms/lmns-${packageJSONVersion}.zip\' --output lmns-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o lmns-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
                }
            }
        }
        
        stage('Clean Up Workspace') {
            steps {
                echo 'Cleaning Workspace'
                cleanWs()
            }
        }
    }
}