pipeline{
  agent any
  stages{
    stage('Checkout'){
      steps{
          // // bat "git branch -D staging"
          // bat "git branch -D staging"
          // bat "git branch staging"
          // bat "git checkout staging"
          // bat "git push origin staging"
          withCredentials([sshUserPrivateKey(credentialsId: "github_ssh", keyFileVariable: 'key', passphraseVariable: 'SSH_PASS')]) {
            // bat "git config --global user.username '$GITHUB_USER'"
            bat "GIT_SSH_COMMAND='ssh -i $key -o StrictHostKeyChecking=no'"
            bat "git branch -D staging"
            bat "git branch staging"
            bat "git checkout staging"
            // bat "git push origin staging"
            bat "echo $SSH_PASS | ssh-add $key - && git push git@github.com:IhabJ/github-jenkins.git staging"
          }
        }
      }

    stage('Build'){
      steps{
        bat "pip3 install -r requirements.txt"
      }
      }

    stage('Test'){
      steps{
        bat "python -m unittest"
        }
      }

    stage('Deploy'){
      steps{
        bat "docker build -t jenkinslab ."
        bat "docker run -d jenkinslab"
        bat "docker tag jenkinslab:latest ihabj/jenkinslab:latest"
        bat "docker push ihabj/jenkinslab:latest"
        }
      }

    stage('Merge'){
      steps{         
          bat "git checkout main"
          bat "git fetch origin"                    
          bat "git merge origin/staging -m 'Merging changes from staging branch'"
          bat "git pull origin main"
          bat "git push origin main"
        }
      }
    }
}

