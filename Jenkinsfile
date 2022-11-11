node {
    stage("Git checkout"){
        git branch: 'main', url: 'https://github.com/suraj4devops/Jenkins-Ansible-K8s-Project.git'
    }
    stage ("sending Docker file to ansible over SSH"){
        sshagent(['jenkins-slave']) {
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138'
            sh 'scp /var/lib/jenkins/workspace/jenkins-ansible-kubernetes/* jenkins@192.168.30.138:/home/jenkins/Desktop'
        }
    }
    stage("Docker Build Stage"){
        sshagent(['jenkins-slave']){
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 cd /home/jenkins/Desktop'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        }
         
    }
    stage("Docker Image tagging and pushing to hub"){
        sshagent(['jenkins-slave']){
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 cd /home/jenkins/Desktop'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker image tag $JOB_NAME:v1.$BUILD_ID suraj435/$JOB_NAME:v1.$BUILD_ID'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker push suraj435/$JOB_NAME:v1.$BUILD_ID'
        //    sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 docker push suraj435/$JOB_NAME:latest'
        }
    }
    stage("copy the Manifest file and Run on k8s master"){
        sshagent(['jenkins-slave']){
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 cd /home/jenkins/Desktop'
            sh 'ssh -o StrictHostKeyChecking=no jenkins@192.168.30.138 ansible-playbook k8s-playbook'
        }
    }
}     
