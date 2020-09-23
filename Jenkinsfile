pipeline {
 
  environment {
    registry = "192.168.1.81:5000/ruetech"
    dockerImage = ""
  
  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/yabsambaye/odoo.git'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("odoo:13")
                    myapp1 = docker.build("postgres:9.4}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                            myapp1.push
                            myapp1.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "odoo.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
