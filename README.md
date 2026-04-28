# Streamlit Calculator CI/CD Demo

This is a simple calculator application built using Streamlit.
It is used to demonstrate Continuous Integration and Continuous Deployment
using Jenkins and GitHub.

## Features
- Addition, Subtraction, Multiplication, Division
- Automated deployment using Jenkins Pipeline
- Poll SCM trigger for CI/CD

## Run Locally
```bash
pip install -r requirements.txt
streamlit run streamlit_app.py

pipeline {
    agent any

    environment {
        APP_NAME = "streamlit-calculator"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/manimegalai-123/Streamlit_Calculator_CICD.git'
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python3 --version
                python3 -m pip install --upgrade pip
                pip3 install streamlit
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "Build stage - checking files"
                ls -l
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "Test stage - basic syntax check"
                python3 -m py_compile app.py
                '''
            }
        }

        stage('Run App') {
            steps {
                sh '''
                echo "Starting Streamlit App"
                nohup streamlit run app.py --server.port 8501 &
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
