// Create podtemplate with label jenkins-hllogmon-agent
// add the pod lable in Cloud kubernetes Configuration jenkins/jenkins-hllogmon-agent
pipeline { 
    agent {
        kubernetes {
            defaultContainer 'jnlp'
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
      readOnly: false
  - name: logmon
    image: chant/habana.ai/hl-log-mon:0.1
    command:
    - sleep
    args:
    - 100
    volumeMounts:
    - name: task-hostpath-storage
      mountPath: /mnt/
      readOnly: false
  volumes:
  - name: task-hostpath-storage
    persistentVolumeClaim:
      claimName: hl-log-pvc
'''           
        }
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                echo params.SN
                echo params.Stage
            }
        }
        stage('Fetch log') { 
            steps {
                container('alpine') {
                    sh 'touch /mnt/chant.test.pvpvc'
                }
            }
        }
        stage('Analyze log') {
            steps {
                container('logmon') {
                    //sh '/home/logmonitor --conf /home/.taipei.json decision --file /mnt/logcsv'
                    sh 'ls /mnt'
                }
            }
        }
    }
}
