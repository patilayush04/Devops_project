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
                git url: "https://github.com/patilayush04/Devops_project.git", branch: "main"
            }
        }

        stage('prepare .env'){
            steps{
                sh '''
                mkdir -p server
cat > server/.env <<EOF
PORT=$PORT
MONGO_URI=$MONGO_URI
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

        stage('use of docker compose'){
            steps{
                sh '''

                echo "starting mern-app using docker compose "
                docker compose up -d

                echo "show all running container"
                docker ps 

                echo " ==== docker build ==="
                docker logs backend || true

                echo " ==== docker build ==="
                docker logs frontend || true

                '''
            }
        }
    }
}