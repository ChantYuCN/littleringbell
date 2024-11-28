pipeline {
    agent any

    stages {
        // stage('Setup Parameter') {
        //     steps {
        //         echo 'Setting..'
        //         script {

        //         }
        //     }
        // }
        stage('Build') {
            steps {
                echo 'Building..'
                echo params.SN
                echo params.STAGE
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
