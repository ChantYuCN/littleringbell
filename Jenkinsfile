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
  - name: alpine
    env:
    - name: HL_CICD_UUID
      value: ${HL_UUID}
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
    env:
    - name: HL_CICD_UUID
      value: ${HL_UUID}
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
                container('alpine') {
                    sh 'echo ${SN} ; echo $HL_CICD_UUID'
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
