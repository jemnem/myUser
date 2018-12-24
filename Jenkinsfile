
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

node ('api-test') {
    stage('Get the code from githut') {
       git credentialsId: 'b6b44b1f-71e5-49b9-89b6-fb29ae993b4e', url: 'https://github.com/jemnem/myUser.git'
    }
    stage('Deploy') {
        echo 'start deploying...'
        sh 'docker-compose --no-ansi stop'
        sh 'docker-compose --no-ansi rm -f'
        // sh 'docker run -v ${PWD}/test:/usr/src/test  -w /usr/src/test/scripts python python util.py set_sql'
        // sh 'cp ${PWD}/test/scripts/data/dump1.sql ${PWD}/docker/catalogue-db/data/'
        sh 'docker-compose --no-ansi build'
        sh 'docker-compose --no-ansi up -d'
        // sleep 120
    }
    stage('Test') {
        echo ' start testing...'
        try {
            // sh 'docker run -v ${PWD}/test:/etc/newman -t postman/newman_ubuntu1404 run postman/catalogue.postman_collection.json -e postman/catalogue_environment.json --color off'
            // sh 'docker run -v ${PWD}/test:/etc/newman -t postman/newman_ubuntu1404 run postman/catalogue_wwei.postman_collection.json -e postman/catalogue_environment.json --color off'
            // sh 'docker run -v ${PWD}/test:/etc/newman -t postman/newman_ubuntu1404 run postman/catalogue_using_external_json_wwei.postman_collection.json -e postman/catalogue_environment.json -d scripts/data/sock.json -n 10 --color off'
        }
        catch (e) {
            echo 'rollback the service to the last good one'
        }

    }
}

