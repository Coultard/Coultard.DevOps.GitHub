# Configure KubeCtl

To configure kubectl:

1. download manually from the control panel.

2. Copy the file to server e.g.scp k8s-1-22-8-do-1-lon1-1654606834405-kubeconfig.yaml philc@dionysus:/home/philc/

3. Move to relevant dir:
mkdir /home/githubdevops/.kube
sudo mv k8s-1-22-8-do-1-lon1-1654606834405-kubeconfig.yaml /home/githubdevops/.kube/config

4. Apply:
su githubdevops
cd ~/
kubectl --kubeconfig=.kube/config get nodes

kubectl get nodesshould output:  
NAME                   STATUS   ROLES    AGE   VERSION 
pool-0zca2eksz-7jh2e   Ready    <none>   52d   v1.22.13 
pool-0zca2eksz-7jh2g   Ready    <none>   52d   v1.22.13 

Check current context:  
kubectl config current-context