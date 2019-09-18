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
        
        
            sh "docker build ."
       
    }
        stage("Docker push") {
           
        sh "docker login -u psk4040 -p psk4040"
        sh "docker push test-0.0.1-SNAPSHOT.jar"
          
        }
        stage("Deploy to staging") {
          
        
                sh "docker run -d --group-add docker -v $(pwd)/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts"
          
        }
      
        
        stage ('Final') {
            build job: 'sample pipeline', wait: false
        }      
 
    }
 
}
