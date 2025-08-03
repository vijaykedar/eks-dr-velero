pipeline {
    agent any

    triggers {
        // Run daily at 2:00 AM
        cron('0 2 * * *')
    }

    environment {
        BACKUP_NAME = "jenkins-backup-${new Date().format('yyyyMMdd-HHmm')}"
 //      NAMESPACE = "your-target-namespace"  // Optional: limit the backup
    }

    stages {
        stage('Velero Backup') {
            steps {

               withKubeConfig(caCertificate: '', clusterName: 'EKS-Demo', contextName: '', credentialsId: 'eks-token', namespace: '', restrictKubeConfigAccess: false, serverUrl: 'https://9AC0B452E60113A6CB7C022E2BDA37D2.gr7.ap-south-1.eks.amazonaws.com') {
    // some block
               
                    sh '''
                    echo "Starting Velero backup: $BACKUP_NAME"

                    velero create backup $BACKUP_NAME \
                      --wait

                    echo "Backup $BACKUP_NAME complete."
                    '''
                }
            }
        }
    }

post {
        success {
            echo "✅ Velero backup completed successfully: $BACKUP_NAME"
            // Optional: send notification
            // slackSend color: 'good', message: "Backup successful: $BACKUP_NAME"
        }
        failure {
            echo "❌ Velero backup failed: $BACKUP_NAME"
            // Optional: send alert
            // slackSend color: 'danger', message: "Backup failed: $BACKUP_NAME"
        }
        always {
            echo "📦 Velero backup job finished at: ${new Date()}"
            // Optional: archive logs
            // archiveArtifacts artifacts: 'backup-log.txt', onlyIfSuccessful: false
        }
    }


}
