pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('pmd') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://debuuguuer', 'teedy') {
                        docker.image(env.DOCKER_IMAGE).push("latest")
                    }
                }
            }
        }
    }

    post{
        always {
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
        }
    }
}