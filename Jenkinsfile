pipeline {
    agent { docker 'node:6.3' }
    environment {
        DISABLE_AUTH          = 'true'
        DB_ENGINE             = 'DynamoDB'
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID') # this is pulled from .env file
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY') # also pulled from .env file
    }
    parameters {
        string{ name: 'Ryan', defaultRegion: 'us-west-2', description: 'This is a description'}
    }
    triggers {
        cron('H 4/* 0 0 1-5')
    }
    stages {
        stage('build') {
            when {
                branch 'test'
            }
            steps {
                echo 'In the test branch..'
                echo 'Inside the build stage..'
            }
        }
        stage('deploy') {
            steps {
                echo 'in the deploy stage..'
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        echo 'retrying deployment 5 times or until 3 minutes causes a timeout'
                        echo 'scripts that do things for deployment would go here..'
                    }
                }
            }
        }
        stage('Any name I want') {
            steps {
                retry(2) {
                    echo 'this is a meaningful step'
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            junit 'coverage/cobertura-coverage.xml' # this should grab the cobertura xml report to display when jenkins runs
        }
        success {
            echo 'This will run only if successful'
            mail to: 'ryanjonesirl@gmail.com',
                subject: "Successful Pipeline: ${currentBuild.fullDisplayName}",
                body: "The pipeline has built successfully!!"
        }
        failure {
            echo 'This will run only if failed'
            slackSend channel: "#ops-room",
                color: 'bad',
                message: "The pipeline ${currentBuild.fullDisplayName} failed!"
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
