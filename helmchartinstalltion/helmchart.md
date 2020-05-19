#### Install helm
```
$ wget https://get.helm.sh/helm-v2.14.1-linux-amd64.tar.gz
$ tar zxf helm*gz
$ sudo cp linux-amd64/helm /usr/local/bin/
$ rm -rf helm* linux-amd64
$ kubectl -n kube-system create serviceaccount tiller
$ kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
$ helm init --service-account tiller # create tiller replica and pods

# if we are getting error like  while run helm init 
error:- HELM_HOME has been configured at /root/.helm.
		Error: error installing: the server could not find the requested resource
solution:- 
   $ helm init --service-account tiller --override spec.selector.matchLabels.'name'='tiller',spec.selector.matchLabels.'app'='helm' --output yaml | sed 's@apiVersion: extensions/v1beta1@apiVersion: apps/v1@' | kubectl apply -f -

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



