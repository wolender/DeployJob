pipeline {
    agent {
        label 'agent1'
    }

    stages {
        stage('Docker Login') {
            steps {
                script{
                      // Find the container ID of the container running on port 80
                      def old_container_id = sh(returnStdout: true, script: "docker ps -q --filter \"publish=80\"").trim()
            
                      // Stop the old container
                      if (old_container_id) {
                        sh "docker stop $old_container_id"
                      }
                      sh 'docker login'
                      sh 'docker pull wolender/release_repo:$VERSION'
                      sh 'docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic -p 3306:3306 -d mysql:8.0'
                      sh 'docker run -p 80:8080 -e DB_MODE=$DB_MODE -d wolender/release_repo:$VERSION '
                }
                
            }
        }
    }
}
