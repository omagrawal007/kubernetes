
# followed url https://octetz.com/docs/2019/2019-03-26-ha-control-plane-kubeadm/
# https://www.youtube.com/watch?v=27v36t-3afQ&t=67s
1. SSH to the master0 host.
2. mkdir /etc/kubernetes/kubeadm  # Create the directory /etc/kubernetes/kubeadm
3. vim /etc/kubernetes/kubeadm/kubeadm-config.yaml # Create and edit the file /etc/kubernetes/kubeadm/kubeadm-config.yaml.
4. Add the following configuration.
----------------------------------------------------------------------------------
  apiVersion: kubeadm.k8s.io/v1beta1
kind: ClusterConfiguration
kubernetesVersion: stable
# REPLACE with `loadbalancer` IP
controlPlaneEndpoint: "192.168.122.170:6443"
networking:
  podSubnet: 192.168.0.0/18
---------------------------------

5. Alter the line with a REPLACE comment above it.
6. Initialize the cluster with upload-certs and config specified.
-----------------------------------------
kubeadm init \
    --config=/etc/kubernetes/kubeadm/kubeadm-config.yaml \
    --experimental-upload-certs
-----------------------------------------
7. Record the output regarding joining control plane nodes for later use.
-------------------------------------------
output:- 
You can now join any number of the control-plane node running the following command on each as root:

kubeadm join 192.168.122.170:6443 --token nmiqmn.yls76lcyxg2wt36c \
--discovery-token-ca-cert-hash sha256:5efac16c86e5f2ed6b20c6dbcbf3a9daa5bf75aa604097dbf49fdc3d1fd5ff7d \
--experimental-control-plane --certificate-key 828fc83b950fca2c3bda129bcd0a4ffcd202cfb1a30b36abb901de1a3626a9df
-----------------------------------------------
8. As your user, run the recommended kubeconfig commands for kubectl access.

------------------------------------------------
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
--------------------------------------------------
9. kubectl get secrets -n kube-system kubeadm-certs -o yaml # Examine the kubeadm-cert secret in the kube-system namespace.
10. kubeadm token list # List available tokens with kubeadm.

----------------------------------------------------------------------
output:

TOKEN TTL EXPIRES USAGES DESCRIPTION
cwb9ra.gegoj2eqddaf3yps 1h 2019-03-26T19:38:18Z Proxy for managing TTL for the kubeadm-certs secret nmiqmn.yls76lcyxg2wt36c 23h 2019-03-27T17:38:18Z authentication,signing

> Note that `cwb9ra` is the owner reference in the above step. This is
> **not** a join token, instead a proxy that enables ttl on `kubeadm-certs`.
> We still need to use the `nmiqmn` token when joining.
----------------------------------------------------------------------
11. Install calico CNI-plugin with a pod CIDR matching the podSubnet configured above.
kubectl apply -f https://gist.githubusercontent.com/joshrosso/ed1f5ea5a2f47d86f536e9eee3f1a2c2/raw/dfd95b9230fb3f75543706f3a95989964f36b154/calico-3.5.yaml
12. kubectl get nodes # Verify 1 node is Ready.
13. kubectl get pods -n kube-system # Verify kube-system pods are Running.


14. SSH to the master1 host.

kubeadm join 192.168.122.170:6443 --token nmiqmn.yls76lcyxg2wt36c \
--discovery-token-ca-cert-hash sha256:5efac16c86e5f2ed6b20c6dbcbf3a9daa5bf75aa604097dbf49fdc3d1fd5ff7d \
--experimental-control-plane \
--certificate-key 828fc83b950fca2c3bda129bcd0a4ffcd202cfb1a30b36abb901de1a3626a9df
15. kubectl get nodes # After completion, verify there are now 2 nodes.

  

