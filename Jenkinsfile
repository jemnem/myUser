
properties([
	buildDiscarder(
		logRotator(
			artifactDaysToKeepStr: '', 
			artifactNumToKeepStr: '', 
			daysToKeepStr: '7', 
			numToKeepStr: '7'
		)
	)
])

timestamps{
	node ('api-test') {
		stage('Get the code from githut') {
		   git credentialsId: 'b6b44b1f-71e5-49b9-89b6-fb29ae993b4e', url: 'https://github.com/jemnem/myUser.git'
		}
		stage('Deploy') {
			echo 'start deploying...'
			sh 'docker-compose --no-ansi stop'
			sh 'docker-compose --no-ansi rm -f'
			sh 'docker-compose --no-ansi build'
			sh 'docker-compose --no-ansi up -d'
			// sleep 120
		}
		stage('Test') {
			echo ' start testing...'
			try {
				//
				// Usage: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
				//
				// Run a command in a new container
				// Options:
				//   -t, --tty							Allocate a pseudo-TTY
				//       --ulimit ulimit				Ulimit options (default [])
				//   -v, --volume list					Bind mount a volume (default [])
				//       --volume-driver string			Optional volume driver for the container
				//       --volumes-from list			Mount volumes from the specified container(s) (default [])
				//   -w, --workdir string				Working directory inside the container
				
				sh 'docker run -v ${PWD}/test:/etc/newman -t postman/newman_ubuntu1404 run postman/user_ocean.postman_collection.json -e postman/user_ocean.postman_environment.json --color off'
			}
			catch (e) {
				echo 'rollback the service to the last good one'
			}

		}
	}
}

