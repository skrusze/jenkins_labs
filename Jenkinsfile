pipeline {
  agent any

  options {
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps {
        echo "Workspace: ${env.WORKSPACE}"
        checkout scm
      }
    }

    stage('Tests') {
      steps {
        sh '''
  set -e
  python3 --version
  python3 -m venv .venv
. .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
pytest -q --junitxml=pytest.xml
'''

      }
    }

    stage('Artifacts') {
      steps {
        archiveArtifacts artifacts: 'pytest.xml', fingerprint: true
      }
    }
  }
}
