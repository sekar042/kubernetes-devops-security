pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }   
       stage('Unit Test') {
            steps {
              sh "mvn test"
              }
               post {
               always {
                 junit 'target/surefire-reports/*.xml'
                 jacoco execPattern: 'target/jacoco.exec'
                 }
               }  
       }
         stage('Docker image build and Push'){
           steps {
             sh 'docker build -t docker-registry:5000/java-app:latest .'
             sh 'docker push docker-registry:5000/java-app:latest'
           }
         }
    }
}
}
