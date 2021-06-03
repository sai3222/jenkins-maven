pipeline {
    environment { 

        registry = "devopshint/my-app-1.0" 

        registryCredential = 'devopshint' 

    }
    agent any
    tools {
        maven 'MAVEN'
    }

    stages {
        stage('Build Maven') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_REPO', url: 'https://github.com/devopshint/jenkins-maven.git']]])

                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
        }
     stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t devopshint/my-app-1.0:latest .'
                }
            }
        }
    stage('Deploy Image') {
        steps {
            script {
                docker.withRegistry( '', registryCredential ) {
                 sh 'docker push devopshint/my-app-1.0:latest'

}
}
}
}
}
}