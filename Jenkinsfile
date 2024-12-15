pipeline {
    agent any
    environment {
        INPUT_FILE = 'test.txt'
    }

    stages {
        stage('Build') {
            steps {
                cleanWs()
                sh '''
                    echo "Hello World"
                    mkdir test_dir
                    touch test_dir/test.txt
                    echo "hello world - Second pipeline" >> test_dir/$INPUT_FILE
                    cat test_dir/test.txt 
                    echo "hello world - Second pipeline-Two" >> test_dir/$INPUT_FILE
                    cat test_dir/$INPUT_FILE
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'python:3.9-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo 'Testing is in progress.....'
                    python --version
                    touch test_dir/test1.txt
                    test -f test_dir/$INPUT_FILE   
                    grep "Two" test_dir/$INPUT_FILE
                    python main.py
                '''
            }
        }
    }
    post {
        success {
            archiveArtifacts artifacts: 'test_dir/**'
        }
    }
}