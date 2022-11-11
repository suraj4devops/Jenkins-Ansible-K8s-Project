node {
    stage("Git checkout"){
        git branch: 'main', url: 'https://github.com/suraj4devops/Jenkins-Ansible-K8s-Project.git'
    }
    stage ("sending Docker file to ansible over SSH"){
        sshagent(['Ansible-Server']) {
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138'
            sh 'scp /var/lib/jenkins/workspace/jenkins_ansible_kubernetes/* jenkins@192.168.30.138:/home/jenkins/Desktop'
        }
    }
    stage("Docker Build Stage"){
        sshagent(['Ansible-Server']){
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 cd /home/jenkins/Desktop'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker image build -t $JOB_NAME:v$BUILD_ID /home/jenkins/Desktop/.'
        }
         
    }
    stage("Docker Image tagging and pushing to hub"){
        sshagent(['Ansible-Server']){
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 cd /home/jenkins/Desktop'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker image tag $JOB_NAME:v$BUILD_ID suraj435/$JOB_NAME:v$BUILD_ID'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker push suraj435/$JOB_NAME:v$BUILD_ID'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker push suraj435/$JOB_NAME:latest '
        }
    }
}     
