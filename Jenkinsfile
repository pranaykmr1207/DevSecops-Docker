pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven_new"
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'
            }
        }
           stage('Test') {
            steps {
                sh "ls ; mvn test"
            }
            post{
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco (execPattern: 'target/jacoco.exec')
                  }
             }
        }
           stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsId: "docker-hub", url: "https://quay.io/"]) {
                sh 'printenv'
                sh 'sudo docker build -t quay.io/pranaykmr/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push quay.io/pranaykmr/numeric-app:""$GIT_COMMIT""'
            }
         }
      }
   }   
}
