def targetMail = "bnagaraju96756@gmail.com,jenkinsautomationuser@gmail.com"
def emailDevOps(targetMail, msg) {
    emailext body: "${msg}\nplease look into the build: ${BUILD_URL}", subject: "${currentBuild.result}: ${BUILD_TAG}", to: "${targetMail}"
}
pipeline {
  agent {label "slave"}
stages {
  stage("build") {
    steps {
      sh "pwd"
      sh "ls -lrtha"
      dir("spring-petclinic") {
      sh "./mvnw clean package"
      }
    }
  }
  stage("deploy") {
    steps {
      dir("spring-petclinic") {
    sh """
    docker build -t pet-clinic:1.0 .
    docker ps -q --filter name=pet-clinic_container|grep -q . && (docker stop pet-clinic_container && docker rm pet-clinic_container) ||echo pet-clinic_container doesn\\'t exists
    docker run --name pet-clinic_container -d -p 8080:8080 pet-clinic:1.0
    """
      }
    }
  }
}
post {
    always{
         deleteDir()
         emailDevOps(targetMail, "This is the mail notification.")
         
    }
}
}
