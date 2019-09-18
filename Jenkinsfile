node {
 
    withMaven(maven:'maven') {
 
        stage('Checkout') {
            git url: 'https://github.com/4040/spring-boot-sample.git', credentialsId: 'github-4040', branch: 'master'
        }
     
      stage('Initialize'){
        def dockerHome = tool 'docker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
 
        stage('Build') {
            sh 'mvn clean install'
             def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
        }
 
     
        stage("Docker build") {
        
        
            sh "docker build -t psk4040/springbootsample."
       
    }
        stage("Docker push") {
           
        sh "docker login -u psk4040 -p psk4040"
        sh "docker push psk4040/springbootsample"
          
        }
        stage("Deploy to staging") {
          
        
                sh "docker run -d --rm -p 8765:8080 --name springbotosample psk4040/springbootsample"
          
        }
      
        
        stage ('Final') {
            build job: 'sample pipeline', wait: false
        }      
 
    }
 
}
