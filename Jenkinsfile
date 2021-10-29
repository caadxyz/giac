def crosscompilers = '/var/lib/jenkins/cross-compilers'

pipeline {
  agent {label 'deploy2'}
  environment {
    PATH="$crosscompilers/x86/bin:$crosscompilers/x86_64/bin:$crosscompilers/arm/bin:$crosscompilers/arm64/bin:$PATH"
    MAVEN = credentials('maven')
    MAC = credentials('mac-giac')
    NPM = credentials('npm-registry')
    LINUX32_SSH=credentials('linux32-ssh-key')
    ANDROID_SDK_ROOT='/var/lib/jenkins/.android-sdk'
    BINARYEN="${env.WORKSPACE}/emsdk/upstream"
    EMSDK_PYTHON='/usr/bin/python3.8'
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          ./gradlew downloadEmsdk installEmsdk activateEmsdk
          ./gradlew :emccClean :giac-gwt:publish --no-daemon -Prevision=56100 --info --refresh-dependencies
          ./gradlew :updateGiac --no-daemon -Prevision=56100 --info'''
      }
    }
  }
  post {
    always {
      cleanAndNotify()
    }
  }
}
