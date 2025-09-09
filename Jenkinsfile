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
        stage('artifact'){
            steps{
                dir('.vitepress/dist'){
                    sh 'ls -la'
                    sh 'tar -zcvf docs.tar.gz *'
                    archiveArtifacts artifacts: 'docs.tar.gz',
                                     allowEmptyArchive: true,
                                     fingerprint: true,
                                     onlyIfSuccessful: true
                    sh 'ls -la'
                }
            }
        }
        stage('deploy'){
            steps{
                dir('.vitepress/dist'){
                    sh 'ls -la'
                    writeFile file: 'Dockerfile',
                              text: '''From nginx ADD docs.tar.gz /usr/share/html'''
                    sh 'cat Dockerfile'
                    sh 'docker build -f Dockerfile -t docs-app:latest .'
                    sh 'docker rm -f app'
                    sh 'docker run -d -p 80:80 --name app docs-app:latest'
                }
            }
        }
    }
}