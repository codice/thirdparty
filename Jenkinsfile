pipeline {
    agent { label 'linux-small' }
    options {
            buildDiscarder(logRotator(numToKeepStr:'25'))
            disableConcurrentBuilds()
            timestamps()
            skipDefaultCheckout()
    }
    environment {
        MVN_OPTS = '-Xmx1024M -Xss128M -XX:+CMSClassUnloadingEnabled -XX:+UseConcMarkSweepGC '
        LINUX_MVN_RANDOM = '-Djava.security.egd=file:/dev/./urandom'
    }
    stages {
        stage('Build') {
            steps {
                retry(3) {
                    checkout scm
                }
                timeout(time: 20, unit: 'MINUTES') {
                    withMaven(maven: 'M35', jdk: 'jdk8-latest', globalMavenSettingsConfig: 'default-global-settings', mavenSettingsConfig: 'codice-maven-settings', mavenOpts: '${MVN_OPTS} ${LINUX_MVN_RANDOM}') {
                        sh 'mvn clean install'
                    }
                }
            }
        }
        /*
          Deploy stage will only be executed for deployable branches. These include master and any patch branch matching M.m.x format (i.e. 1.0.x, 1.1.x, etc...).
          It will also only deploy in the presence of an environment variable JENKINS_ENV = 'prod'. This can be passed in globally from the jenkins master node settings.
        */
        stage('Deploy') {
            when {
                allOf {
                    expression { env.CHANGE_ID == null }
                    expression { env.BRANCH_NAME ==~ /((?:\d*\.)?\d*\.x|master)/ }
                    environment name: 'JENKINS_ENV', value: 'prod'
                }
            }
            steps{
                withMaven(maven: 'M3', jdk: 'jdk8-latest', globalMavenSettingsConfig: 'default-global-settings', mavenSettingsConfig: 'codice-maven-settings', mavenOpts: '${LINUX_MVN_RANDOM}') {
                    sh 'mvn deploy -DretryFailedDeploymentCount=10'
                }
            }
        }
    }
}
