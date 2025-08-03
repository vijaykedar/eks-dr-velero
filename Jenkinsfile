pipeline {
    agent any

    triggers {
        // Run daily at 2:00 AM
        cron('0 2 * * *')
    }

    environment {
        BACKUP_NAME = "jenkins-backup-${new Date().format('yyyyMMdd-HHmm')}"
 #       NAMESPACE = "your-target-namespace"  // Optional: limit the backup
    }

    stages {
        stage('Velero Backup') {
            steps {
                script {
                    sh '''
                    echo "Starting Velero backup: $BACKUP_NAME"

                    # Optional: restrict to a namespace, or remove --include-namespaces to back up all
                    velero create backup $BACKUP_NAME \
                    #  --include-namespaces $NAMESPACE \
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
