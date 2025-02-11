pipeline {
    agent any

    environment {
        ALLURE_RESULTS = 'allure-results'   // Directory where allure results are stored
        ALLURE_REPORT = 'allure-report'     // Directory where allure report will be generated
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies
                sh 'mvn clean install -DskipTests'  // If you want to skip tests during the build
            }
        }

        stage('Run Tests') {
            steps {
                // Run the tests using Maven and Allure Reporter
                sh 'mvn clean test'  // This will trigger the tests and generate allure results
            }
        }

        stage('Generate Allure Report') {
            steps {
                // Generate Allure Report using the Allure Maven plugin
                sh 'mvn allure:serve'  // This will generate the Allure Report
            }
        }

        stage('Publish Allure Report') {
            steps {
                allure includeProperties: false, jdk: '', results: [[path: "${ALLURE_RESULTS}"]]
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after the build
        }
    }
}