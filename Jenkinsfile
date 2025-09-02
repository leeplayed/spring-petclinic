pipeline {
  agent any

  stages{
    stage('Git Clone'){
      steps {
        git url: 'https://github.com/leeplayed/spring-petclinic.git', branch: 'main'
      }
    }
    stage('Maven Build'){
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
      }
    }
  }
}
