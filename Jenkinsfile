def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'custom-docker', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'jenkins', image: 'jenkins/jnlp-slave:3.10-1', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/home/gradle/.gradle', hostPath: '/tmp/jenkins/.gradle'),
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
    node(label) {
        echo "Your Pipeline works!"
        stage('Build') {
          try {
            container('gradle') {
            }
          }
          catch (exc) {
            println "Failed to test - ${currentBuild.fullDisplayName}"
            throw(exc)
          }
        }
        stage('Test') {
          container('gradle') {
            sh "gradle build"
          }
        }

        sh('ls -la')
        sh('npm install')
        sh('npm test')
    }
  }
