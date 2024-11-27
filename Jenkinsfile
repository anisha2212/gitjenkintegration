pipeline {
    agent any
    
    triggers {
        pollSCM('H/5 * * * *') // Poll every 5 minutes
    }


    stages {
        stage('Checkout') {
            steps {
                git branch: 'test', url: 'https://github.bmc.com/IZOT-UAP/End-to-end-CI-CD-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                // Assuming calc.txt is the REXX program
                echo 'Building the REXX program'
                bat 'cat CALC.txt > build.log'  // Replace with actual build steps if needed
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                // Simulate running the REXX program with input and capturing output
                bat 'ex "EXECIO * DISKR INPUTS.txt (STEM INPUT_FILE. FINIS)"; ex "EX CALC.txt"; ex "EXECIO * DISKW OUTPUT.txt (STEM OUTPUT_FILE. FINIS)"'

                // Validate the output
                bat 'VALID.txt OUTPUT.txt'  // Replace with actual validation script command
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to test branch'
                // Push changes to GitHub if tests pass
                script {
                    def changes = sh(script: 'git diff --name-only', returnStdout: true).trim()
                    if (changes) {
                        bat 'git add .'
                        bat 'git commit -m "Automated commit: Updated output"'
                        bat 'git push origin test'
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/OUTPUT.txt', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
