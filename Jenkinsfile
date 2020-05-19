pipeline {
    agent any
    stages {
        stage('Build') {
	    environment {
                CONFLUENCE_CREDS=credentials('katsdpdocstest')
            }
            steps {
                sh '''#!/bin/bash
		  FILES=($(git show --name-only --diff-filter=ACMRT | grep '.md$'))
		  echo "Processing files for possible Confluence upload: $FILES"
		  echo "Test ls"
                  echo `docker run --rm -i -v /var/jenkins_home:/var/jenkins_home -v=$(pwd):/docs sdp-docker-registry.kat.ac.za:5000/docker-base-build ls /docs`
		  echo `docker run --rm -i -v /var/jenkins_home:/var/jenkins_home -v=$(pwd):/docs sdp-docker-registry.kat.ac.za:5000/docker-base-build ls /var/jenkins_home`
			
		  if [[ "${#FILES[@]}" == "0" ]]; then exit 0; fi;
		  set -e;
		  for file in ${FILES[@]}; do
		    echo;
		    echo "> Uploading ${file}";
		    docker run --rm -i -v=`pwd`:/docs kovetskiy/mark:latest -l https://skaafrica.atlassian.net/wiki -u CONFLUENCE_CREDS_USR -p CONFLUENCE_CREDS_PSW -f ${file};
		  done
                '''
            }
        }
    }
}
