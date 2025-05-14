#!groovy

pipeline {
    agent {
        docker {
            image 'renovate/renovate:40.11.10'
            args '-v /tmp:/tmp --group-add 0 --entrypoint=sh'
        }
    }

    environment {
        CONFIGURATION = 'Release'
        TZ = 'Europe/Berlin'
        ENV = '/usr/local/etc/env'
        RENOVATE_TOKEN = credentials('RENOVATE_TOKEN')
        GITHUB_COM_TOKEN = credentials('GITHUB_COM_TOKEN')
        LOG_LEVEL = 'debug'
        GIT_AUTHOR_NAME = 'Renovate Bot'
        GIT_AUTHOR_EMAIL = 's.aqajjef@gmail.com'
        GIT_COMMITTER_NAME = 'Renovate Bot'
        GIT_COMMITTER_EMAIL = 's.aqajjef@gmail.com'
        RENOVATE_PLATFORM = 'github'
        //RENOVATE_ENDPOINT = 'https://git.example.com/api/v4/'
        RENOVATE_REPOSITORIES = 'saqajjef/flask-gitops'
        RENOVATE_ONBOARDING_CONFIG = '{ "extends":["config:base"] }'
    }

    parameters {
        string defaultValue: '', description: '', name: 'RENOVATE_EXTRA_FLAGS', trim: true
    }

    options {
        //gitLabConnection('git.example.com')
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
        ansiColor('xterm')
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '240')
    }

    triggers {
        cron('H * * * *')
    }

    stages {
        stage('init') {
            steps {
                sh 'renovate --version'
                sh 'rm -f renovate.log'
            }
        }

        stage('renovate') {
            steps {
                sh "renovate --log-file renovate.log --log-file-level debug ${params.RENOVATE_EXTRA_FLAGS}"
            }
        }
    }

    post {
        always {
            archiveArtifacts allowEmptyArchive: true, artifacts: 'renovate.log'
        }
    }
}
