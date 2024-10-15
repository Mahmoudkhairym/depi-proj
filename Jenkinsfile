pipeline {
    agent any

    stages {
        stage('prep') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/Mahmoudkhairym/pro.git'
            }
        }
        stage('build'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerid2', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    // Run Maven on a Unix agent.
                    sh 'docker build -t ${USER}/app -f Dockerfile .'
                    sh 'docker login -u ${USER} -p ${PASS}'
                    sh "docker push ${USER}/app"
                }
            }
        }
        stage ('prod'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerid2', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker rm -f ${USER}/app'
                    sh 'docker run -d -p 8000:8000 --name contapp ${USER}/app'
                }
            }
        }
    }
}
