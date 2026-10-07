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
                echo 'Deploying application to GitHub Pages...'

                withCredentials([usernamePassword(
                    credentialsId: 'github-token',
                    usernameVariable: 'GITHUB_USER',
                    passwordVariable: 'GITHUB_TOKEN'
                )]) {

                    bat '''
                    git config user.name "Jenkins"
                    git config user.email "jenkins@localhost"

                    git clone https://%GITHUB_USER%:%GITHUB_TOKEN%@github.com/duggg911/food-delivery-app.git deploy-repo

                    copy /Y index.html deploy-repo\\
                    copy /Y style.css deploy-repo\\
                    copy /Y README.md deploy-repo\\

                    cd deploy-repo
                    git add index.html style.css README.md
                    git commit -m "Deploy website from Jenkins" || echo No changes to commit
                    git push origin master
                    '''

                    echo 'Deployment to GitHub Pages completed successfully.'
                }
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