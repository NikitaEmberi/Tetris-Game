# React Tetris V1

Tetris game built with React

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/game.jpg" />
</h1>

- Deployed multiple versions of a Tetris game on AWS EKS using Terraform for infrastructure setup and Jenkins for CI/CD pipeline automation.
- Implemented Argo CD for continuous delivery, enabling efficient deployment and version management of containerized applications.
- Established a complete CI/CD pipeline that included building Docker images, pushing to Docker Hub, and managing application updates via GitHub.
- Achieved seamless automation of application deployment processes, enhancing the efficiency of development workflows.

Use Sonarqube block 
```
environment {
        SCANNER_HOME=tool 'sonar-scanner'
      }

stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Amazon \
                    -Dsonar.projectKey=Amazon '''
                }
            }
        }
```        

Owasp block
```
stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
```

# ARGO CD SETUP
https://archive.eksworkshop.com/intermediate/290_argocd/install/

# Image updater stage
```
 environment {
    GIT_REPO_NAME = "Tetris-manifest"
    GIT_USER_NAME = "Aj7Ay"
  }
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/Aj7Ay/Tetris-manifest.git'
      }
    }

    stage('Update Deployment File') {
      steps {
        script {
          withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
            // Determine the image name dynamically based on your versioning strategy
            NEW_IMAGE_NAME = "sevenajay/tetris77:latest"

            // Replace the image name in the deployment.yaml file
            sh "sed -i 's|image: .*|image: $NEW_IMAGE_NAME|' deployment.yml"

            // Git commands to stage, commit, and push the changes
            sh 'git add deployment.yml'
            sh "git commit -m 'Update deployment image to $NEW_IMAGE_NAME'"
            sh "git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main"
          }
        }
      }
    }

```
Link to Version 2 repository: https://github.com/NikitaEmberi/Tetris-Game-v2

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/img3.png" />
</h1>

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/img4.png" />
</h1>

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/img5.png" />
</h1>

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/img6.png" />
</h1>

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/img7.png" />
</h1>

<h1 align="center">
  <img alt="React tetris " title="#React tetris desktop" src="./images/img8.png" />
</h1>

CREDITS TO: [Article](https://aakibkhan1.medium.com/project-9-deployment-of-tetris-game-on-kubernetes-and-automating-it-with-argo-cd-and-terraform-via-7ca8b3068378)

