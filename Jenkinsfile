pipeline {
  agent any

  tools {
    maven "M3"
    jdk "JDK17"
  }

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerCredentials')
  }

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
    stage('Docker Image Create') {
      steps {
        sh """
        docker build -t leeplayed/spring-petclinic:$BUILD_NUMBER .
        docker tag leeplayed/spring-petclinic:$BUILD_NUMBER leeplayed/spring-petclinic:latest
        """
      }
    }
    stage('Docker Hub Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Docker Image Push') {
      steps {
        sh 'docker push leeplayed/spring-petclinic:latest'
      }
    }
    stage('Docker Image Remove') {
      steps {
        sh 'docker rmi leeplayed/spring-petclinic:$BUILD_NUMBER leeplayed/spring-petclinic:latest'
      }
    }
stage('SSH Publish') {
            steps {
                echo 'SSH Publish'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'web01', 
                transfers: [sshTransfer(cleanRemote: false, 
                excludes: '', 
                execCommand: '''
                fuser -k 8080/tcp
                export BUILD_ID=Petclinic-Pipeline
                nohup java -jar /home/ubuntu/deploy/spring-petclinic-2.7.3.BUILD-SNAPSHOT.jar >> nohup.out 2>&1 &''', 
                execTimeout: 120000, 
                flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: 'target', 
                sourceFiles: 'target/*.jar')], 
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, verbose: false)])
                              echo 'SSH Publish'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'web02', 
                transfers: [sshTransfer(cleanRemote: false, 
                excludes: '', 
                execCommand: '''
                fuser -k 8080/tcp
                export BUILD_ID=Petclinic-Pipeline
                nohup java -jar /home/ubuntu/deploy/spring-petclinic-2.7.3.BUILD-SNAPSHOT.jar >> nohup.out 2>&1 &''', 
                execTimeout: 120000, 
                flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: 'target', 
                sourceFiles: 'target/*.jar')], 
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, verbose: false)])
            }
        }
  }
}
