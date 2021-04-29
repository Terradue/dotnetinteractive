pipeline {
  agent any
  environment {
      VERSION_TYPE = getTypeOfVersion(env.BRANCH_NAME)
  }
  stages {
    stage('Build & Publish Docker') {
      steps {
        script {
          def descriptor = readDescriptor()
          def mType=getTypeOfVersion(env.BRANCH_NAME)
          def testsuite = docker.build(descriptor.docker_image_name + ":${mType}" + descriptor.docker_image_version, ".")
          testsuite.tag("${mType}latest")
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-emmanuelmathot') {
            testsuite.push("${mType}" + descriptor.docker_image_version)
            testsuite.push("${mType}latest")
          }
        }
      }
    }    
  }
}

def getTypeOfVersion(branchName) {
  def matcher = (branchName =~ /master/)
  if (matcher.matches())
    return ""
  
  return "dev"
}

def readDescriptor (){
  return readYaml(file: 'build.yml')
}



