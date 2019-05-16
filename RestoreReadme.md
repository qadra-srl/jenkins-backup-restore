## Restore Jenkins

1. Download ThinBackup Plugin
2. Download AWS: Step Plugin
3. Configure credentials
4. Run a backup with ThinBackup specifying:
    - ${JENKINS_HOME}/jenkins-backups as backup folder
    - H 20 * * * as interval
    - -1 as max number of backups
    - Tick options 1,2,5,6
5. Run a backup
6. Restore previous version
7. Restart {jenkinsUrl}/safeRestart
8. Reset credentials
9. Install aws cli on slave through docker exec
    - apt-get install unzip
    - curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
    - unzip awscli-bundle.zip
    - ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
10. Install jq on slave through docker exec