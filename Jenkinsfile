node {
 
    withMaven(maven:'maven') {
 
        stage('Checkout') {
            git url: 'https://github.com/4040/spring-boot-sample.git', credentialsId: 'github-4040', branch: 'master'
        }
         stage('Initialize')
    {
        def dockerHome = tool 'dockerengine'
        env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
    }
 
        stage('Build') {
            sh 'mvn clean install'
             def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
        }

  
        stage('Image') {
         //   dir ('discovery-service') {
                def app = docker.build "localhost:5000/:${env.version}"
                app.push()
          //  }
        }
 
        stage ('Run') {
            docker.image("localhost:5000/:${env.version}").run('-p 8761:8761 -h sample --name sample')
        }
 
        stage ('Final') {
            build job: 'sample pipeline', wait: false
        }      
 
    }
 
}
