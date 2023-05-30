pipeline {
    agent any
    stages {       
        stage('git') {
              steps {
                git branch: 'main', url: 'https://github.com/Rambabu1969/new-test-jenkins.git'
            }
        }
         stage('deploy on Remote Server (http://13.233.112.233/102)') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'LinuxWebServer', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/var/www/html/102', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        stage('deploy in S3 Bucket') {
            steps {
                s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'mru-coding-club-vivily-2111cs010159', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'ap-south-1', showDirectlyInBrowser: false, sourceFile: '**/**', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'MRU_AppDevProjects', userMetadata: []
            }
        }
         stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
