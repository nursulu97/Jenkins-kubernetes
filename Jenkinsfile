template = '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: docker
  name: docker
spec:
  containers:
  - command:
    - sleep
    - "3600"
    image: docker
    name: docker
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker
  volumes:
  - name: docker
    hostPath:
      path: /var/run/docker.sock
    '''

podTemplate(cloud: 'kubernetes', label: 'docker', yaml: template) {
    node ("docker") {
        container ("docker") {
    stage ("Checkout SCM docker"){
       git branch: 'main', url: 'https://github.com/nursulu97/Jenkins-kubernetes.git'
    }
withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USERNAME')]) {
    stage ("Dockerbuild") {
        sh "docker build -t ${DOCKER_USERNAME}/apache:2.0 ."
    }

    stage ("Docker push") {
        sh """
        docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASS}
        docker push ${DOCKER_USERNAME}/apache:2.0
        """
    
   
    }
}
    

        }
    }
}
