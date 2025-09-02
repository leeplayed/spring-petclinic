pipeline {
  agent any

  stages{
    stage('Git Clone'){
      steps {
        git url: 'https://github.com/leeplayed/spring-petclinic.git', branch: 'main'
      }
    }
  }
}
