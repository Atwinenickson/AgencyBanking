pipeline {
    agent any
    tools {nodejs "Node"}
    environment {
        CI = 'true'
        API_DIR = '/var/lib/jenkins/workspace/AGENCY'
        DEV_ENV = 'dev'
    }
    stages {
        stage('Preparation') {
            steps{
                git branch: "master",
                url: 'https://github.com/Atwinenickson/AgencyBanking.git',
                credentialsId: 'root'
            }
        }
        stage('Deploy to AGENCY Dev') {
            environment{
                RETRY = '80'
            }
            steps {
             echo 'Create a dev environment'
             sh '/home/atwine/Pictures/apictl/apictl version'
                sh '/home/atwine/Pictures/apictl/apictl list envs'
                sh '/home/atwine/Pictures/apictl/apictl remove env dev'
                sh '/home/atwine/Pictures/apictl/apictl add-env -e  $DEV_ENV --apim https://192.168.0.113:9443'
                echo '--------------------Logging into $DEV_ENV----------------'
				withCredentials([usernamePassword(credentialsId: 'apim_dev', usernameVariable: 'DEV_USERNAME', passwordVariable: 'DEV_PASSWORD')]) {
                    sh '/home/atwine/Pictures/apictl/apictl login $DEV_ENV -u $DEV_USERNAME -p $DEV_PASSWORD -k'                        
                }   
                echo '------------------------Deploying to $DEV_ENV-----------------------'
                sh '/home/atwine/Pictures/apictl/apictl import-api -f $API_DIR -e $DEV_ENV -k --preserve-provider --update --verbose'
            }
        }
    }
}
