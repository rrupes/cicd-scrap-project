pipeline {
  agent any

  triggers {
    pollSCM('* * * * *')  // 🔄 Triggers the pipeline if there's a code change in GitHub
  }

  environment {
    SSH_KEY = '/var/lib/jenkins/.ssh/id_rsa'       // ✅ Jenkins private SSH key path (to connect to Ansible server)
    ANSIBLE_USER = 'root'                          // 👤 Username for Ansible server login
    ANSIBLE_IP = '3.81.158.244'                    // 🌐 Ansible server's public IP
    REMOTE_PATH = '/root/cicdk8'                 // 📁 Folder in Ansible server to copy code into
  }

  stages {

    stage('Clone') {
      steps {
        git 'https://github.com/rrupes/cicd-scrap-project.git'
      }
    }

    stage('Test Trigger') {
      steps {
        echo '✅ Build triggered by GitHub push or pollSCM'
      }
    }

    stage('Copy to Ansible Server') {
      steps {
        sh '''
          echo "📦 Copying project code to Ansible server..."

          # ✅ Make sure the remote folder exists
          # example for below line ssh -i /var/lib/jenkins/.ssh/id_ed25519 root@3.81.158.244  
          ssh -i $SSH_KEY $ANSIBLE_USER@$ANSIBLE_IP "mkdir -p $REMOTE_PATH"

          # ✅ Copy current project code to Ansible server
          # -r → recursive, meaning copy entire directories.
          scp -i $SSH_KEY -r . $ANSIBLE_USER@$ANSIBLE_IP:$REMOTE_PATH

          echo "✅ Code successfully copied to Ansible server at $REMOTE_PATH"
        '''
      }
    }
    // stage('Ansible Deploy') {
    //   steps {
    //    sh '''
    //     echo "🚀 Running Ansible playbook to build & push Docker images..."
    //     ssh -i $SSH_KEY $ANSIBLE_USER@$ANSIBLE_IP "ansible-playbook $REMOTE_PATH/ansible/playbook.yml"
    //   '''
    //   }
    }

}

