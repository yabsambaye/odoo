pipeline {

  environment {
    registry = "37.59.48.5:5000/yabsambaye/myweb"
    dockerImage = ""
  }
 
  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/yabsambaye/odoo.git'
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
                            myapp.push
                            myapp1.push
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
