pipeline {
     agent any
     environment {
          DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-howthegodschill')
     }
     stages {
          stage("Compile") {
               steps {
                    sh "./gradlew compileJava"
               }
          }
          stage("Unit Test") {
               steps {
                    sh "./gradlew test"
               }
          }
          stage("Code Coverage") {
               steps{
                   sh "./gradlew jacocoTestReport"
                   publishHTML (target: [
                         reportDir: 'build/reports/jacoco/test/html',
                         reportFiles: 'index.html',
                         reportName: "JaCoCo Report"
                   ])
                   sh "./gradlew jacocoTestCoverageVerification"
               }
          }
          stage ("Static Code Analysis" ) {
               steps {
                    sh "./gradlew checkstyleMain"
                    publishHTML (target: [
                         reportDir: 'build/reports/checkstyle',
                         reportFiles: 'main.html',
                         reportName: "Checkstyle Report"
                    ])
               }
          }
          stage("Package Application") {
               steps{
                   sh "./gradlew build"
               }
          }
          stage("Docker Build") {
               steps{
                   sh "docker build -t howthegodschill/calculator ."
               }
          }
          stage("Docker Login") {
               steps{
                   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
               }
          }
          stage("Docker Push") {
               steps{
                   sh "docker push howthegodschill/calculator"
               }
          }
     }
}
