node('slave') {
    git branch: 'main' ,url: 'https://github.com/dendilz/demo.git'
    env.GIT_COMMIT_NAME = getCommit()
    env.APP_PATH = "/var/www/symfony"
    
    if(!fileExists("$APP_PATH/site-$GIT_COMMIT_NAME")){
        stage ('Build'){
            sh "chmod +x ./bin/console"
            sh "composer install"
        }
        stage ('Deploy'){
            sh "mkdir $APP_PATH/site-$GIT_COMMIT_NAME"
            sh "cp -r . $APP_PATH/site-$GIT_COMMIT_NAME"
            sh "ln -fsn $APP_PATH/site-$GIT_COMMIT_NAME $APP_PATH/site"
            sh "chmod -R 777 $APP_PATH/site-$GIT_COMMIT_NAME"
        }
        stage ('Remove old builds'){
            sh "cd $APP_PATH/ && ls -tp | tail -n +5 | xargs rm -rf"
        }
        stage ('Save artifacts') {
            archiveArtifacts artifacts: 'public/**/*.*', fingerprint: true
        }
    }    
}

String getCommit() {
    return sh(script: """git rev-parse HEAD | tail -c 7""", returnStdout: true)?.trim()
}
