parameters = [
  string(name: 'plateforme', description: "Nom plateforme", defaultValue: ""),
  string(name: 'branch',     description: "Branche du repository common", defaultValue: "master"),
  string(name: 'app_name',   description: "Nom de l'application", defaultValue: ""),
  string(name: 'app_version',   description: "Version de l'application", defaultValue: ""),
]
properties([ parameters(parameters) ])

def cmdInVirtualEnv(cmd) {
  sh(". $WORKSPACE/python/bin/activate > /dev/null 2>&1 ; ${cmd}")
}

def betterCallAnsible(parameters) {
  print("Lancement d'ansible sur l'inventaire ${params.plateforme}")
  cmdInVirtualEnv("ansible --version")
}

node() {
  stage('install') {
    sh("virtualenv $WORKSPACE/python")
    cmdInVirtualEnv("pip install ansible mitogen --upgrade")
  }
  stage('init') {
    dir('common') {
      git(branch: params.branch, /* credentialsId: 'my-cred', */ url: 'https://github.com/Yannig/juc-jenkins-2018.git')
    }
  }
  stage('socle') {
    betterCallAnsible("playbooks/common/ce-que-tu-veux.yml")
  }
}
