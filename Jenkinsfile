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
        stage('DeployToStaging'){
        when{
            branch 'master'
        }
        steps{
            withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
            sshPublisher(
                continueOnError: false, failOnError: true,
                publishers: [
                sshPublisherDesc(
					configName: 'staging',
					sshCredentials:[
						username: "$USERNAME",
						encryptedPassphrase: "$PASSWORD"
					],
					transfers: [
						sshTransfer(
							sourceFiles: 'dist/trainSchedule.zip',
							removePrefix: 'dist/',
							remoteDirectory: '/tmp',
							execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/'
						)
					]
				)
				]
			)
        }
		}
	}
}
