pipeline {
    agent any

    parameters {
        choice(name: 'environment', description: 'Select an environment', choices: ['prod', 'devel', 'cat'])
        string(name: 'imageName', description: 'Docker image name')
        string(name: 'tagInput', description: 'tag docker image')
        string(name: 'nautobotName', description: 'nautobot docker image name and tag - example "networktocode/nautobot:stable-py3.8"')
    }

    environment {
        imageNameInput = ''
        tagInput = ''
    }

    stages {
        stage('Download nautobot docker image') {
            steps {
                script {
                    def nautobotImage = docker.image("${params.nautobotName}")
                    nautobotImage.pull()
                }
            }
        }

        stage('Tag docker image') {
            steps {
                script {                   
                    def newTag = "nautobot-${params.environment}.nlhrkgdttarfc01.atos-srv.net/admin/${params.imageName}:${params.tagInput}"
                    sh "docker tag ${params.nautobotName} ${newTag}"
                }
            }
        }

        stage('Push docker image') {
            steps {
                script {
                    docker.withRegistry("https://nautobot-${params.environment}.nlhrkgdttarfc01.atos-srv.net") {
                        def nautobotTaggedImage = docker.image("nautobot-${params.environment}.nlhrkgdttarfc01.atos-srv.net/admin/${params.imageName}:${params.tagInput}")
                        nautobotTaggedImage.push()
                    }
                }
            }
        }
    }
}
