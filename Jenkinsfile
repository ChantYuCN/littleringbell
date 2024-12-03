// Create podtemplate with label jenkins-hllogmon-agent
// add the pod lable in Cloud kubernetes Configuration jenkins/jenkins-hllogmon-agent
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                echo params.SN
                echo params.STAGE
            }
        }
        stage('fetch log') {
            agent {
                kubernetes {
                    yaml '''
kind: Pod
spec:
    containers:
    - name: logmon
      image: chant/habana.ai/hl-log-mon:0.1
      command:
      - sleep
      args:
      - 100
      volumeMounts:
      - name: task-hostpath-storage
        mountPath: /mnt/
    volumes:
    - name: task-hostpath-storage
      hostPath:
      path: /home/labuser/habanashared
      type: Directory
'''
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
