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
          python3 --version || true
          # Testy odpalamy w kontenerze python, żeby nie zależeć od Pythona w Jenkinsie:
          docker run --rm -v "$PWD":/work -w /work python:3.12-slim \
            sh -lc "pip install -r requirements.txt && pytest -q --junitxml=pytest.xml"
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
