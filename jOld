def commit_id
pipeline {
    agent any
    

    stages {
        stage('Preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit-id"
                script {                        
                    commit_id = readFile('.git/commit-id').trim()
                }
            }
        }
        stage('Build') {
            steps {
                echo '1-----------------------Building the maven project--------------------------'
                sh 'mvn clean install'
                echo 'build complete'
            }
        }
        stage('Image Build') {
            steps {
                echo '2--------------------Building image-------------------'
                sh "'docker build -t position-simulor:${commit_id} ./assignment_6/'"
                echo 'build complete'
            }
        }
        stage('Deploy') {
            steps {
                echo '3-----------------------Deploying to Kubernetes------------------------'
                sh "sed -i -r 's|richardchesterwood/k8s-fleetman-position-simulator:release2|position-simulator:${commit_id}|' workloads.yaml"
                sh 'kubectl get all'
                sh 'kubectl apply -f .'
                sh 'kubectl get all'
                echo 'deployment complete'
                
            }
        }
    }
  }
