parameters = [
  string(name: 'plateforme', description: "Nom plateforme", defaultValue: ""),
  string(name: 'branch',     description: "Branche du repository common", defaultValue: "master"),
  string(name: 'app_name',   description: "Nom de l'application", defaultValue: ""),
  string(name: 'app_version',   description: "Version de l'application", defaultValue: ""),
  booleanParam(name: 'disableInstall', description: "Désactiver l'installation", defaultValue: false),
]
properties([ parameters(parameters) ])

ansibleEnv = [
  "ANSIBLE_STRATEGY=mitogen_linear",
  "ANSIBLE_CALLBACK_WHITELIST=profile_tasks,timer",
  "ANSIBLE_STRATEGY_PLUGINS=${env.WORKSPACE}/lib/python2.7/site-packages/mitogen/ansible_mitogen/plugins/strategy"
]

def cmdInVirtualEnv(cmd) {
  sh(". $WORKSPACE/python/bin/activate > /dev/null 2>&1 ; ${cmd}")
}

def betterCallAnsible(parameters) {
  print("Lancement d'ansible sur l'inventaire ${params.plateforme}")
  withEnv(ansibleEnv) {
    cmdInVirtualEnv("ansible --version")
  }
}

node() {
  stage('install') {
    if(!params.disableInstall) {
      sh("virtualenv $WORKSPACE/python")
      cmdInVirtualEnv("pip install ansible mitogen --upgrade")
    }
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
