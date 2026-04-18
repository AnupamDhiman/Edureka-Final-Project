pipeline {
    agent any

    tools {
        maven 'Maven-3'   // Configure this name in Jenkins Global Tool Configuration
        jdk 'JDK-8'       // Configure this name in Jenkins Global Tool Configuration
    }

    environment {
        DOCKER_HUB_REPO = 'anupamdhiman/abctechnologies'
        DOCKER_CREDENTIALS = 'dockerhub-credentials'  // Jenkins credentials ID for Docker Hub
        KUBE_CONFIG = credentials('kubeconfig')        // Jenkins credentials ID for kubeconfig
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Pulling source code from GitHub...'
                git branch: 'main',
                    url: 'https://github.com/AnupamDhiman/Edureka-Final-Project.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo '🔨 Building application with Maven...'
                sh 'mvn clean package -DskipTests=false'
            }
            post {
                success {
                    echo '✅ Build successful!'
                    archiveArtifacts artifacts: 'target/*.war', fingerprint: true
                }
                failure {
                    echo '❌ Build failed!'
                }
            }
        }

        stage('Code Coverage') {
            steps {
                echo '📊 Generating JaCoCo code coverage report...'
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    jacoco(
                        execPattern: 'target/jacoco.exec',
                        classPattern: 'target/classes',
                        sourcePattern: 'src/main/java'
                    )
                }
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Building Docker image...'
                script {
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo '📤 Pushing image to Docker Hub...'
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS) {
                        dockerImage.push("${BUILD_NUMBER}")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '☸️ Deploying to Kubernetes cluster...'
                sh """
                    kubectl --kubeconfig=${KUBE_CONFIG} apply -f Deployment.yml
                    kubectl --kubeconfig=${KUBE_CONFIG} apply -f Service.yml
                    kubectl --kubeconfig=${KUBE_CONFIG} rollout status deployment/test1 --timeout=120s
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '✅ Verifying deployment...'
                sh """
                    kubectl --kubeconfig=${KUBE_CONFIG} get pods -l app=test1
                    kubectl --kubeconfig=${KUBE_CONFIG} get svc svc1
                """
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '🚨 Pipeline failed! Check the logs above for errors.'
        }
        always {
            echo '🧹 Cleaning up workspace...'
            cleanWs()
        }
    }
}
