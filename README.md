## Jenkins master
kubectl create ns jenkins
kubectl -f jenkins-deploy-service.yaml
kubectl get all -n jenkins (look for the pod, you need it for the password)
kubectl logs pod/jenkins-XXXXXXXXXXXX -n jenkins (search for the ------------------)
#-> maanage jenkins -> users -> change admin pass
#-> manage jenkins -> Configure Global Security -> uncheck (Prevent Cross Site Request Forgery exploits)
#-> manage jenkins -> plugin manager -> 
    - kubernetes
    - workflow-job
    - workflow-aggregator
    - credentials-binding
    - git
    - blueocean
    - kubernetes-cd
    - thinBackup
# ------ create configmap for each kubernetes cluster ------
#create config for kubectl from your kubernetes cloud / rancher....
kubectl create configmap aws-qa-us-east aws-qa-us-east-config.yaml -n jenkins
# ------ Create / Add nodes -------
#login to the Jenkins master: admin / PASS from logs
#-> manage jenkins 
#-> manage nodes
# ------ Create template node ------
#-> new node
#-> Name: template  (Permanent Agent)
#-> # of executors 4
#-> Remote root directory /home/jenkins
#-> Labels docker (Save)
# ------ Create node ------
#-> new node
#-> name: agent001
#-> Copy Existing Node template
# ------ to create a node use the helm (you can edit the valus or use --set)
helm upgrade --install jenkins-agent005 ./jenkins-agent
