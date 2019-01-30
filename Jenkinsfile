#!groovy
@Library("ace") _

ace([dockerSet: false]) {
  stage('Helm Lint') {
    def groovylintImage = 'abletonag/groovylint'
    def groovylintVersion = 'latest'
    def groovylintOpts = "--entrypoint=''"

    docker.image("${groovylintImage}:${groovylintVersion}").inside(groovylintOpts) {
      sh '''
        python3 /opt/run_codenarc.py \
         -report=console \
         -includes="**/*.groovy"
      '''
    }
  }

  stage('Push') {
    dockerPush()
  }

  stage('Deploy') {
    deploy('test')

    slack.notifyDeploy('test')
  }
}
