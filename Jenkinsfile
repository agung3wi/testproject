node {
    checkout scm

    // build
    stage("Build"){
        docker.image('zzidzz/php-composer:8.2-alpine').inside('-u root') {
            sh 'rm composer.lock'
            sh 'composer install'
        }
    }

    // test
    stage("Test"){
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    // deploy
    stage("Deploy"){
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent (credentials: ['ssh-dev']) {
                sh 'mkdir -p ~/.ssh'
                sh 'ssh-keyscan -H "bimo-dev.pttas.net" > ~/.ssh/known_hosts'
                sh "rsync -rav --delete ./ agung@bimo-dev.pttas.net:/home/agung/bimo-dev.pttas.net/ --exclude=.env --exclude=storage --exclude=.git"
                sh "ssh agung@bimo-dev.pttas.net 'cd /home/agung/bimo-dev.pttas.net/ && php8.2 artisan migrate'"
            }
        }
    }
}
