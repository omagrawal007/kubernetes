$ kubectl create ns finance # create namespcaes in kubernetes
$ kubectl get namespcaes
$ openssl genrsa -out john.key 2048 # generate rsa key using openssl 2048 is bit  
$ ls # check john.key  # this is private key
$ openssl req -new -key john.key -out john.csr -subj "/CN=john/0=finance"  #create a cert signing request 
$ ls # check csr file
Important Note
-----------------------------------------------------------------
 1. Now we got the private key and certificate siging request. In order to sign on certificate signing request we need certificate authurity and private key
 2. Go kubernetes master and go inside /etc/kubernetes/pki . you can see there are two file one is ca.crt,ca.key(private)
 3. these file require for  signing on john.csr (certificate signing request)
 4. copy those two file to where we kept john.csr  
 -----------------------------------------------------------------
 $  openssl x509 -req -in john.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out john.crt -days 365     #sign on john.csr command
 $ ls # check john.crt file availabe?
Note
------------------------------------------------------------------
   this certificate will valid till 365
   ca.key,ca.crt  # we copied this file from kubernet pki folder
   we will get john.crt file on same location
  
  Keep all file . all file are important
  
 After get john.crt file we need to create kubectl config file.
 we can create config file in two way
  1. john it self create config file:-
     i) we should send three file to john. (ca.crt,john.crt,john.key)
	 ii) kubectl --kubeconfig john.kubeconfig config set-cluster kubernetes --server https://172.42.42.100:6443 --certificate-authurity=ca.crt
	 iii) kubectl config view   # see server: 172.42.2.100
	 Now we need to add user
	 IV) $ kubectl --kubeconfig john.kubeconfig config set-credentials john --client-certificate /home/om/john.crt --client-key john.key
	 Now need to set context
	 v) $ kubectl --kubeconfig john.kubeconfig config set-context john-kubernetes --namespcaes finance --user john
	 VI) $ kubectl --kubeconfig john.kubeconfig(filewithpath) get pods
	 vII)
	 
  2. we can create config for john:- 
     run $ cat john.crt | base64 -w0 
	 
    i) we can copy our config file from kubelet.conf file from /etc/kubernetes
      $ cat john.key  | base64 -w0
	ii) copy john.crt like client-certificate-data: xxsdfadsfasdfxxasdfasdf
        copy john.key like  client-key-data: asdfxccjaskdfjklasdlfaldhfkjhkkjkjkkhklhj

   we can kubectl.conf file to john 
   john can check like this
   $ kubectl --kubeconfig john.kubeconfig(filewithpath) get pods
		
------------------------------------------------------------------

certificate work done. we should create role 
$ kubectl create role john-finance --verb= get,list --resource=pods --namespcaes finance

$ kubectl -n finance get role john-finance -o yaml
$ kubectl create rolebinding john-finance-rolebinding --role=john-finance --user=john --namespcaes finance
$ kubectl -n finance get rolebinding john-finance-rolebinding -o yaml

