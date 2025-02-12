pipeline {
    agent any
   tools {
         maven 'Maven'
     }
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
                sh 'mvn allure:report'  // This will generate the Allure Report
            }
        }

       stage('Publish Allure Report') {
            steps {
                script {
                    // Publish Allure report
                    allure([
                        includeProperties: false,
                        jdk: '',
                        reportBuildPolicy: 'ALWAYS',
                        results: [[path: 'target/allure-results']]
                    ])
                }
            }
        }
    }

    post {
       always {
                   script {
                       def allureUrl = "${env.BUILD_URL}allure/"

                       emailext subject: "Allure Test Raporu - Build #${env.BUILD_NUMBER}",
                                body: """
                                Merhaba,<br><br>
                                Son çalıştırılan pipeline için Allure raporuna aşağıdaki linkten ulaşabilirsiniz:<br>
                                <a href="${allureUrl}">${allureUrl}</a>
                                <br><br>
                                İyi çalışmalar.
                                """,
                                to: "msozenth93@gmail.com",
                                mimeType: 'text/html'
                   }
               }

    }
}