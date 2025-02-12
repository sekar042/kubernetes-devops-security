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
         stage('Docker Build and Push'){
           steps {
             withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
             sh 'printenv'
             sh 'docker build -t tochandru/numeric-app:""$GIT_COMMIT"" .'
             sh 'docker push tochandru/numeric-app:""$GIT_COMMIT""'
             }
           }
         }
    stage('Kubernetes Dployment - Dev'){
      steps {
        withKubeConfig([credentialsId: 'kubeconfg']){
          sh "sed -i 's#replace#tochandru/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          sh "kubectl apply -f k8s_deployment_service.yaml"
        }
    }
    }
}
}
