@NonCPS
def sortFiles(files) {
    return files.sort{ it.lastModified }
}

node('master') {
    try{
        stage 'Push to S3'
            sh "ls ${JENKINS_HOME}/${BACKUP_FOLDER}/"
            sh "mkdir tmp"
            sh "cp -rT ${JENKINS_HOME}/${BACKUP_FOLDER}/ tmp"
            sh "ls"
            sh "tar -czvf ${BACKUP_FOLDER}.tar.gz -C tmp ."
            sh "echo Uploading to ${S3_BUCKET} "
            withAWS(credentials:"${AWS_CREDENTIALS}", region: "${AWS_REGION}") {
                    s3Upload(file:"${BACKUP_FOLDER}.tar.gz", bucket: "${S3_BUCKET}", path:"jenkins-configuration-${BUILD_ID}.tar.gz")
            }
    }
    catch(e){ throw e}
    finally{
        stage 'Cleanup'
            sh "rm -rf tmp"
            sh "rm -rf ${BACKUP_FOLDER}.tar.gz"

        // stage "Remove old backup files"
        //     withAWS(credentials:"${AWS_CREDENTIALS}", region: "${AWS_REGION}") {
        //        files = s3FindFiles(bucket: "${S3_BUCKET}")

        //        def sortedFiles = sortFiles(files)
        //        def filesToDelete = sortedFiles.size() - 10
        //        for (file in sortedFiles) {
        //             filesToDelete -= 1
                    
        //             if(filesToDelete >= 0){
        //                 println "Deleting ${file.name} from S3"
        //                 s3Delete(bucket:"${S3_BUCKET}", path:file.name)
        //             }
        //         }
        //     }
    }
}