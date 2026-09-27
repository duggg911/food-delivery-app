pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building Food Delivery App...'
                bat 'if exist index.html (echo Build successful) else (echo Build failed & exit /b 1)'
                bat 'if exist style.css (echo CSS file found) else (echo CSS file missing & exit /b 1)'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Food Delivery App...'
                bat 'findstr /C:"Food Delivery App" index.html'
                bat 'findstr /C:"Veg Thali" index.html'
                bat 'findstr /C:"Paneer Biryani" index.html'
                echo 'All tests passed.'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application...'
                bat 'powershell -Command "Compress-Archive -Path index.html,style.css,README.md -DestinationPath food-delivery-app.zip -Force"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat 'if not exist C:\\JenkinsDeploy mkdir C:\\JenkinsDeploy'
                bat 'copy /Y food-delivery-app.zip C:\\JenkinsDeploy\\'
                echo 'Deployment completed successfully.'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}