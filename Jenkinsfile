// Create podtemplate with label jenkins-hllogmon-agent
// add the pod lable in Cloud kubernetes Configuration jenkins/jenkins-hllogmon-agent
pipeline { 

    environment {
        HL_UUID = UUID.randomUUID().toString()
    }
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yaml '''
kind: Pod
spec:
  containers:
  - name: curla
    image: curlimages/curl:8.11.0
    command:
    - sleep
    args:
    - 6
    volumeMounts:
    - name: task-hostpath-storage
      mountPath: /mnt/
      readOnly: false
  - name: logmon
    image: chant/habana.ai/hl-log-mon:0.2
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
                echo env.HL_UUID
            }
        }
        stage('Fetch log') { 
            steps {
                container('curla') {
                    sh 'ls '
                }
            }
        }
        stage('Analyze log') {
            steps {
                container('logmon') {
                    //sh '/home/logmonitor --conf /home/.taipei.json decision --file /mnt/logcsv'
                    sh 'env'
                }
            }
        }
    }
}
