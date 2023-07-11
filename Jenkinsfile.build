pipeline {
  agent {
    kubernetes {
      yaml """
      apiVersion: v1
      kind: Pod
      metadata:
        labels:
          jenkins: maven
      spec:
        containers:
        - name: maven
          image: maven:3.8.4
          command:
          - cat
          tty: true
        - name: docker
          image: docker:dind
          command:
          - dockerd-entrypoint.sh
          tty: true
          securityContext:
            privileged: true
        - name: helm
          image: alpine/helm:3.5.4
          command:
          - sleep
          - infinity
      """
    }
  }

  stages {

    // Checkout the WebGoat Repository
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/WebGoat/WebGoat.git'
      }
    }
    
    // Build using Maven
    stage('Build') {
      steps {
        container('maven') {
            sh 'mvn clean install -DskipTests'
        }
      }
    }
    
    // Test using Maven
    stage('Test') {
      steps {
        container('maven') {
            sh 'mvn test'
        }
      }
    }

    // Run Sonar Scan
    // stage('SonarScan') {
    //   steps {
    //     container('sonar') {
    //         echo 'Executing Sonar Scan...'
    //     }
    //   }
    // }

    // Build Image using Docker in Docker
    stage('Build Docker Image') {
      steps {
        container('docker') {
          sh 'docker build -t webgoat .'
        }
      }
    }

    // Push to GCR
    // stage('Push image to GCR') {
    //   steps {
    //     container('docker') {
    //       echo 'Pushing image to GCR'
    //     }
    //   }
    // }

    // Build Lint Chart
    stage('Helm Lint Chart') {
      steps {
        container('helm') {
          git branch: 'main', url: 'https://github.com/SamBird/webgoat-build.git'
          sh 'cd ./webgoat-chart'
          sh 'helm lint .'
          sh 'helm package  .'
        }
      }
    }
    
  
  }
}
