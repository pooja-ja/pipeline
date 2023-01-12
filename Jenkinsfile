pipeline {
	agent any

	tools {
		maven "maven"
		jdk "jdk1.8"
	}

	stages {
		stage("fetch the code") {
			steps {
				git "https://github.com/pooja-ja/game-of-life.git"
			}
		}
		stage("build the package") {
			steps {
			   	sh "mvn clean package"
			}			

		}
		stage("copy war file") {
			steps {
			sshagent(['50df8e99-9ba5-4c59-a4fe-3aef0593af1f']) {
			sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/dc-pipeline/gameoflife-web/target/gameoflife.war ec2-user@18.191.203.93:/home/ec2-user/'
			}
		}
		}
		stage("Deploying on container") {
			agent {
				label {
					label "slave-ssh"
				}
			}
			steps {
				sh '''
				sudo yum install docker -y
				sudo systemctl start docker'''
                //sh 'sudo docker rm -f myapp'
                sh 'docker-compose up -d'
                     
			}
		}
	}
}
