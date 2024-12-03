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
        stage('Fetch log') { 
            agent {
                kubernetes {
                    yaml '''
kind: Pod
spec:
  containers:
  - name: alpine
    image: alpine:3.12
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
        }
            steps {
                sh 'echo hellowww'
            }
        }
        stage('Analyze log') {
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
            }
            steps {
                echo 'chant k8s try and error'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
