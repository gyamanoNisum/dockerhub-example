pipeline {
  agent {
    kubernetes {
      label 'tools'
      yaml """
        kind: Pod
        metadata:
          name: jenkinspod
        spec:
          containers:
          - name: tools
            image: 'nexus.mynisum.com:2376/nisum/mpl-cicd:0.1.9'
            command:
            - cat
            tty: true
          - name: kaniko
            image: gcr.io/kaniko-project/executor:debug
            imagePullPolicy: Always
            command:
            - /busybox/cat
            tty: true
            volumeMounts:
              - name: jenkins-docker-cfg
                mountPath: /kaniko/.docker
          volumes:
          - name: jenkins-docker-cfg
            projected:
              sources:
              - secret:
                name: registry-cred
                items:
                  - key: .dockerconfigjson
                    path: config.json
        """
    }
  }
  stages {
    stage('Checkout') {
      steps {
        echo 'Checking out the project repo'
        git branch: 'main',
        url: 'https://github.com/gyamanoNisum/dockerhub-example'
      }
    }
  }
}