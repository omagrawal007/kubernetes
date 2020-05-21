1.  Setup NFS server and check is nfs server working fine?
2.  Install nfs-provisioner using kubernetes . YAML file are availbe in ../yamls/nfs-provisioner
      i) there are FOUR file 1. class.yaml 2. default-sc.yaml 3. deployment.yaml 4. rbac.yaml
         class.yaml file create storage class in kubernetes
         deployment.yaml  create NFS client provision 
         steps:- 1. create role,clusterbinding using rbac.yaml file 
                        kubectl create -f rbac.yaml
                        kubectl get clusterrolebinding,clusterrole, role, rolebinding | grep nfs
                2. create storage class resource using class.yaml
                    kubectl get storageclass
                    kubectl create -f class.yaml
                3. create deployment using deployment.yaml but change the mount nfs ip and storage path  (172.31.28.113)
                    watch kubectl get all
                    kubectl create -f deployment.yaml 
                    

                    
            
