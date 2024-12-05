// Create podtemplate with label jenkins-hllogmon-agent
// add the pod lable in Cloud kubernetes Configuration jenkins/jenkins-hllogmon-agent



pipeline { 

    environment {
        HL_CICD_UUID = UUID.randomUUID().toString()
    }
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yaml '''
kind: Pod
spec:
  containers:
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
                echo env.HL_CICD_UUID
            }
        }
        stage('Fetch log') {
            steps {
                script {
                    def reqBody = """ 
                        {"sn": "$SN","uuid": "$HL_CICD_UUID"}
                    """
                    def response = httpRequest httpMode: 'GET', 
                                    contentType: 'APPLICATION_JSON',
                                    requestBody: reqBody,
                                    url: 'http://10.227.108.26:31911/filepath'
                    println("Content: " + response.content)
                    println(HL_CICD_UUID)
                }
            }
            post {
                success {
                    echo("Folder found  " + params.SN )
                }
                failure {
                    echo("Folder not found  " + params.SN )
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
