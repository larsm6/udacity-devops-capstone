def skipRemainingStages = false

pipeline {
    agent any
    stages {
        stage('Create Kubernetes Cluster') {
            steps {
                script {
                    try {
                        withAWS(
                            region: 'us-west-2',
                            credentials: 'aws-kubectl'
                        ) {
                            sh '''
                                eksctl create cluster \
                                --name udacity-devops-capstone \
                                --version 1.16 \
                                --nodegroup-name capstone-workers \
                                --node-type t2.small \
                                --nodes 2 \
                                --nodes-min 1 \
                                --nodes-max 4 \
                                --node-ami auto \
                                --region us-west-2 \
                                --zones us-west-2a \
                                --zones us-west-2b \
                                --zones us-west-2c \
                            '''
                        }
                    } catch (err) {
                        echo err.getMessage()
                    }
                }
                echo currentBuild.result
            }
        }
        stage('Create Configuration File Cluster') {
            steps {
                withAWS(region:'us-west-2', credentials:'aws-kubectl') {
                    sh '''
                        aws eks --region us-west-2 update-kubeconfig --name udacity-devops-capstone
                    '''
                }
            }
        }
    }
}