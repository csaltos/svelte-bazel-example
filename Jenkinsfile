pipeline {
    agent any

    stages {
        stage('GIT') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'LocalBranch', localBranch: "${GIT_BRANCH_LOCAL}"]], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/thelgevold/svelte-bazel-example.git']]])
            }
        }
        stage('Yarn') {
            steps {
                sh "yarn"
            }
        }
        stage('Build') {
            steps {
                parallel(
                  a: {
                    sh "bazel build //src/frontend:bundle_prod"
                  },
                  b: {
                    sh "bazel build //src/backend:ts_server"
                  }
                )
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}