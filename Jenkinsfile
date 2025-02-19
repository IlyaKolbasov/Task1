pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Извлечение кода из GitHub
                git branch: 'master', credentialsId: 'github-ssh-key', url: 'git@github.com:IlyaKolbasov/Task1.git'
            }
        }
        stage('Build') {
            steps {
                // Сборка приложения
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {

                // Используем SSH-учетные данные для подключения к VPS
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'server',
                        transfers: [
                            sshTransfer(
                                sourceFiles: 'target/kolbasov-task.war',
                                remoteDirectory: 'tomcat/apache-tomcat-9.0.100/webapps/',

                                 execCommand: 'cp /root/tomcat/apache-tomcat-9.0.100/webapps/target/kolbasov-task.war /root/tomcat/apache-tomcat-9.0.100/webapps/'

                            )
                        ],
                        usePromotionTimestamp: false,
                        useWorkspaceInPromotion: false,

                    )
                ])

            }
        }
    }
}