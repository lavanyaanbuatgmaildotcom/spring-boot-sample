node {
    stage 'Clone the project'
    git 'https://github.com/4040/spring-boot-sample.git'
   
    dir('spring-jenkins-pipeline') {
        stage("Compilation and Analysis") {
            parallel 'Compilation': {
                sh "./mvnw clean install -DskipTests"
            }
        }
         
        
    }
}