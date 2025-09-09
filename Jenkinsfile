//jenkins pipeline
pipeline{
    agent any

    stages{
        stage('build'){
            steps{
                withDockerContainer('node'){
                    sh 'ls -la'
                    sh 'node -v'
                    sh 'npm install'
                    sh 'npm run docs:build'
                    sh 'ls .vitepress/'
                }
            }
        }
    }
}