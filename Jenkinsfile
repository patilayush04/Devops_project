pipeline{
    agent any

    environment{
        BACKEND_IMAGE = "mern-backend:jenkins"
        FRONTEND_IMAGE = "mern-frontend:jenkins"
        PORT = "5000"
        MONGO_URI = "mongodb://mongo:27017/project"
    }

    stages{

        stage('checkout repo'){
            steps{
                git url: "https://github.com/patilayush04/Devops_project", branch: "main"

            }
        }

        stage('prepare .env'){
            steps{

                sh '''
                mkdir -p server
cat > server/.env << EOF
MONGO_URI=$MONGO_URI
PORT=$PORT
EOF
'''
            }
        }

        stage('build images'){
            steps{
                sh '''
                echo "build backend image"
                docker build -t $BACKEND_IMAGE ./server

                echo "build frontend image"
                docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
                '''
            }
        }

        stage('use of compose to run app'){
            steps{
                sh '''
                echo "run app using compose"
                docker compose up -d --no-build

                echo "show running containers"
                docker ps

                echo "==== backend logs ===="
                docker logs backend || true

                echo "==== backend logs ===="
                docker logs frontend || true

                '''
            }
        }
    }
}