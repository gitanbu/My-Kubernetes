                 Procedure for remove a master node from kubernetes cluster.
                 **********************************************************

 I am going to remove 192.168.56.142(kubemas3.anbu.com) node from etcd cluster.
 
    List the etcd members
    ---------------------
 
[root@kubemas1 kubelet]# etcdctl member list
9f04057cd10cb0b7, started, 192.168.56.142, https://192.168.56.142:2380, https://192.168.56.142:2379, false
a4ee350447b1c083, started, 192.168.56.144, https://192.168.56.144:2380, https://192.168.56.144:2379, false
afd5b034595cfe09, started, 192.168.56.143, https://192.168.56.143:2380, https://192.168.56.143:2379, false

   Remove a member from cluster
   ----------------------------

[root@kubemas1 kubelet]# etcdctl member remove 9f04057cd10cb0b7

Member 9f04057cd10cb0b7 removed from cluster be7714fdd77450dc

 Reset the node from 192.168.56.142(kubemas3.anbu.com) server.
 ------------------------------------------------------------
 
 [root@kubemas3 etcd]# kubeadm reset
[reset] WARNING: Changes made to this host by 'kubeadm init' or 'kubeadm join' will be reverted.
[reset] Are you sure you want to proceed? [y/N]: y
[preflight] Running pre-flight checks
W0118 18:26:58.230052    1793 removeetcdmember.go:79] [reset] No kubeadm config, using etcd pod spec to get data directory
[reset] No etcd config found. Assuming external etcd
[reset] Please, manually reset etcd to prevent further issues
[reset] Stopping the kubelet service
[reset] Unmounting mounted directories in "/var/lib/kubelet"
[reset] Deleting contents of config directories: [/etc/kubernetes/manifests /etc/kubernetes/pki]
[reset] Deleting files: [/etc/kubernetes/admin.conf /etc/kubernetes/kubelet.conf /etc/kubernetes/bootstrap-kubelet.conf /etc/kubernetes/controller-manager.conf /etc/kubernetes/scheduler.conf]
[reset] Deleting contents of stateful directories: [/var/lib/kubelet /var/lib/dockershim /var/run/kubernetes /var/lib/cni]

The reset process does not clean CNI configuration. To do so, you must remove /etc/cni/net.d

The reset process does not reset or clean up iptables rules or IPVS tables.
If you wish to reset iptables, you must do so manually by using the "iptables" command.

If your cluster was setup to utilize IPVS, run ipvsadm --clear (or similar)
to reset your system's IPVS tables.

The reset process does not clean your kubeconfig files and you must remove them manually.
Please, check the contents of the $HOME/.kube/config file.


 Drain the Node from cluster
 ---------------------------
 
[root@kubemas1 kubelet]# kubectl drain kubemas3.anbu.com
node/kubemas3.anbu.com already cordoned

 Delete the Node from cluster
 ----------------------------

[root@kubemas1 kubelet]# kubectl delete node kubemas3.anbu.com
node "kubemas3.anbu.com" deleted
