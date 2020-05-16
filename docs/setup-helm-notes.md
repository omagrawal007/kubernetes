#### Install helm
```
wget https://get.helm.sh/helm-v2.14.1-linux-amd64.tar.gz
tar zxf helm*gz
sudo cp linux-amd64/helm /usr/local/bin/
rm -rf helm* linux-amd64
kubectl -n kube-system create serviceaccount tiller
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
helm init --service-account tiller

uninstall 
  helm reset
  helm  help reset
  
  kubectl -n kube-system delete rs tiller_rs_name
  kubectl -n kube-system delete serviceaccount tiller
  kubectl -n kube-system delete clusterrolebinding tiller
  rm -rf /usr/local/bin/helm
  rm -rf .helm
```

*Wait for tiller component to be active*



