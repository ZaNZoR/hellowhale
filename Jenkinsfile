pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/ZaNZoR/hellowhale.git', branch:'main'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("zanzornl/hellowhale:${env.BUILD_ID}").toString()
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}").toString()
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
