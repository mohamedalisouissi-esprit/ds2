pipeline {
agent any
tools {
    maven 'Maven-3.9.9' // Assure-toi que ce nom correspond à l'installation dans Jenkins
    jdk 'jdk11'
}

environment {
    SONAR_HOST_URL = 'http://localhost:9000' // Modifie si ton SonarQube a une autre URL
    SONAR_PROJECT_KEY = 'ds2-project'
    DOCKER_IMAGE = 'app:latest'
    GIT_CREDENTIALS = 'github-pat' // Assure-toi que ce credential existe dans Jenkins
}

stages {

    stage('Cloner le dépôt') {
        steps {
            timeout(time: 5, unit: 'MINUTES') {
                git(
                    url: 'https://github.com/mohamedalisouissi-esprit/ds2.git',
                    branch: 'master',
                    credentialsId: "${GIT_CREDENTIALS}"
                )
            }
        }
        post {
            success {
                echo '✅ Code cloné avec succès !'
            }
            failure {
                echo '❌ Échec du clonage du dépôt !'
            }
        }
    }

    stage('Build Maven') {
        steps {
            sh 'mvn clean package -DskipTests'
        }
        post {
            success {
                echo '✅ Build Maven réussi !'
            }
            failure {
                echo '❌ Échec du build Maven !'
            }
        }
    }

    stage('Analyse SonarQube') {
        steps {
            withSonarQubeEnv(installationName: 'sonarqubeconnection', credentialsId: 'sonar-token') {
                sh '''
                mvn sonar:sonar \
                    -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    -Dsonar.host.url=${SONAR_HOST_URL} \
                    -Dsonar.token=$SONAR_AUTH_TOKEN
                '''
            }
        }
        post {
            success {
                echo '✅ Analyse SonarQube réussie !'
            }
            failure {
                echo '❌ Échec de l\'analyse SonarQube !'
            }
        }
    }

    stage('Docker Build') {
        steps {
            script {
                docker.build("${DOCKER_IMAGE}")
            }
        }
        post {
            success {
                echo '✅ Image Docker construite avec succès !'
            }
            failure {
                echo '❌ Échec de la construction de l\'image Docker !'
            }
        }
    }
}

post {
    always {
        echo '🧹 Nettoyage des ressources...'
        cleanWs()
        echo "🕓 Pipeline terminé à ${new Date().format('HH:mm:ss')} CET le ${new Date().format('dd/MM/yyyy')}"
    }
    success {
        echo '🎉 Pipeline terminé avec succès !'
    }
       failure {
        echo '🚫 Échec du pipeline !'
    }
}
}
