without using any yaml file from current foler just go to
kubernetes website and run default yaml file

# port forwording 

$ kubectl -n kubernetes-dashboard get all

# 1. First check dashboard is running in which port
  $ kubectl -n kubernetes-dashboard describe service kubernetes-dashboard 
# kubectl -n kubernetes.dashboard port-forward kuber(replicaset name)
open with https://localhost:8000/#/login

# change clusterip to nodeport
# kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard
# change and save it 

# get token step
  1. $ kubectl -n kubernetes-dashboard get serviceaccount
  2. $ kubectl -n kubernetes-dashboard describe serviceaccount kubernetes-dashboard
  3. copy tokens
  4. $ kubectl -n kubernetes-dashboard describe secret (paste copied token)
# we does not have all permission of admin so create service account and all step given below
  
 #                                   -:Proper idea to create dashboard step below:-


 1.  $ kubectl create -f sa_cluster_admin.yaml
 2. $ kubectl -n kube-system get sa  
 3. $ kubectl -n kube-system describe serviceaccount dashboard-admin
 4. copy tokens
 5.  $ kubectl -n kube-system describe secrete (paste copied token)

