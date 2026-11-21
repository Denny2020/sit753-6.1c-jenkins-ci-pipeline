// SIT753 6.1C - Part 1 Task 1: mock 7-stage CI/CD pipeline.
// Each stage only echoes the task it would perform and the tool that would do it.
pipeline {
    agent any

    // Poll GitHub every ~2 minutes; a build starts only when a new commit is found.
    // Run the job once manually so Jenkins registers this trigger.
    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        APP_NAME           = 'sit753-demo-app'
        STAGING_SERVER     = 'staging-ec2 (AWS EC2, ap-southeast-2)'
        PRODUCTION_SERVER  = 'production-ec2 (AWS EC2, ap-southeast-2)'
    }

    stages {
        stage('Stage 1: Build') {
            steps {
                echo "Task: compile the source and package ${APP_NAME} into a deployable artefact"
                echo 'Tool: Maven (mvn clean package) - alternatives: Gradle, npm'
            }
        }

        stage('Stage 2: Unit and Integration Tests') {
            steps {
                echo 'Task: run unit tests to check each component works as expected'
                echo 'Task: run integration tests to check the components work together'
                echo 'Tools: JUnit (unit), TestNG / Mockito (integration) - for Node.js: Jest, Mocha + Supertest'
            }
        }

        stage('Stage 3: Code Analysis') {
            steps {
                echo 'Task: analyse the code for bugs, code smells and duplication against industry standards'
                echo 'Tool: SonarQube / SonarCloud (via the SonarQube Scanner for Jenkins plugin)'
            }
        }

        stage('Stage 4: Security Scan') {
            steps {
                echo 'Task: scan the code and its dependencies for known vulnerabilities'
                echo 'Tools: OWASP Dependency-Check (dependencies), Snyk or npm audit, Trivy (container image)'
            }
        }

        stage('Stage 5: Deploy to Staging') {
            steps {
                echo "Task: deploy the packaged artefact to the staging server: ${STAGING_SERVER}"
                echo 'Tools: AWS CodeDeploy or Ansible (alternative: Docker + SSH Agent plugin)'
            }
        }

        stage('Stage 6: Integration Tests on Staging') {
            steps {
                echo "Task: run integration / end-to-end tests against ${STAGING_SERVER} (production-like environment)"
                echo 'Tools: Selenium (UI), Postman / Newman (API)'
            }
        }

        stage('Stage 7: Deploy to Production') {
            steps {
                echo "Task: deploy the verified release to the production server: ${PRODUCTION_SERVER}"
                echo 'Tools: AWS CodeDeploy or Ansible (blue/green deployment)'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished with status: ${currentBuild.currentResult}"
        }
    }
}
