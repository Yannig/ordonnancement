parameters = [
  string(name: 'plateforme', description: "Nom plateforme", defaultValue: ""),
  string(name: 'branch',     description: "Branche du repository common", defaultValue: "master"),
  string(name: 'app_name',   description: "Nom de l'application", defaultValue: ""),
  string(name: 'app_version',   description: "Version de l'application", defaultValue: ""),
]
properties([ parameters(parameters) ])

def betteCallAnsible() {
  print("Lancement d'ansible sur l'inventaire ${params.plateforme}")
}

node() {
  stage('install') {
    sh("virtualenv $WORKSPACE/python")
    sh(". $WORKSPACE/python/bin/activate ; pip install ansible mitogen --upgrade")
  }
}
