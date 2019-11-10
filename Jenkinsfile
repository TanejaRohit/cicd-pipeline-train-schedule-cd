pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging')
        when{
            branch 'master'
        }
        steps{
            withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
            sshPublisher(
                continueOnError: false, failOnError: true,
                publishers: [
                sshPublisherDesc(
                configName: "${env.SSH_CONFIG_NAME}",
                verbose: true,
                transfers: [
                sshTransfer(
                    sourceFiles: "${path_to_file}/${file_name}, ${path_to_file}/${file_name}",
                    removePrefix: "${path_to_file}",
                    remoteDirectory: "${remote_dir_path}",
                    execCommand: "run commands after copy?"
                )
            ])
        ])
        }
    }
}
