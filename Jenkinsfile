pipeline{
    agent { label 'default' }
    environment{
        username     = "sidharthvijayakumar"
        repository   = "Test-repo"
        email        = "sidharthvijayakumar7@gmail.com"
        repoDir      = "${env.WORKSPACE}/sidharth-github"
    }
    tools {
        maven 'maven'
    }
    stages{
        stage('Git Clone'){
            steps{
                script {
                    //def repoDir= "${env.WORKSPACE}/sidharth-github"
                    //When you need to invoke environment variable in stage use ${env.VAR_NAME}
                    echo "The ${env.repository} will be cloned to: ${repoDir}" 
                    dir(repoDir){
                        sh "rm -rf $repository"
                        if (!fileExists(env.repository)){
                                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                                    sh '''
                                        #When you need to invoke environment variable in shell block invoke with $VAR_NAME
                                        git clone https://github.com/$username/$repository
                                    '''
                                }
                        }
                    }  
                }                    
            }
        }
        stage('Git push'){
            steps{
                script {
                    //def repoDir= "${env.WORKSPACE}/sidharth-github"
                    dir(repoDir){
                        dir(env.repository){
                            withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                                sh '''
                                    git config --global user.email "$email"
                                    git config --global user.name "$username"
                                    git checkout main
                                    echo "This is file1" > file1.txt
                                    echo "This is file2" > file2.txt
                                    git remote set-url origin https://${username}:${GIT_PASSWORD}@github.com/${username}/${repository}.git
                                    echo "https://$GITHUB_USER:$GITHUB_PAT@github.com" > ~/.git-credentials
                                    git add .
                                    if git diff-index --quiet HEAD; then
                                        echo "No changes to push"
                                        exit 0
                                    else
                                        git commit -m "updated the changes"
                                        git push origin main
                                    fi
                                '''
                            }
                        }
                    }
                }
            }
        }
        stage('maven test'){
            steps{
                script{
                    sh "mvn -v"
                }
            }
        }
    }
}
