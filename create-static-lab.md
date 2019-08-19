
### Background Details:
I have attached a RH Linux CoreOS Eval Guide.  Much of this information was used in creating the OCP Architecture Overview documentation [0].  It also contains explanations and examples of various modifications that can be performed to meet your environment needs.

Additional information on the installation process [1], OCP Control Plane [2], and RHCoS [3] is recommended reading before beginning deployment of OCP 4.x.

### Cluster Requirements:
The OpenShift (OCP) documentation [4] for vSphere lists the various requirements needed to correctly create an OCP 4.x cluster.
  + vSphere 6.7 U2 or later recommended
    + vSphere 6.5 with Hardware version 11 supported
  + Minimal cluster [5]
    + One bootstrap node
    + Three Control Plane/Master nodes
    + 2 Compute/Worker nodes
  + Two Load balancers
  + DNS Entries
  + Internet connectivity (no disconnected install)

#### DNS/Network Requirements
Since we are replacing the DHCP requirement with static ip addresses we need to make sure the servers can fetch the required items during boot and configuration of the node.  This mean the network details will need to be configured in the ignition file and during the boot process.

###### DNS Entries Required: [6]
  + api.<cluster_name>.<base_domain>  
    + points to the load balancer for the control plane
    + must be resolvable external from the cluster and by all nodes in the cluster
  + api-int.<cluster_name>.<base_domain> 
    + points to the load balancer for the control plane
    + must be resolvable from all nodes in the cluster
  + *.apps.<cluster_name>.<base_domain>
    + wildcard dns record that points to the load balancer which targets the nodes running the Ingress Controller (compute by default)
    + must be resolvable exteran from the cluster and by all nodes in the cluster
  + etcd-<index>.<cluster_name>.<base_domain>
    + record for each etcd instance that points to the control plane node
    + index starts at zero, ends with n-1 where n is the number of control plane nodes
    + must be resolvable from all nodes in the cluster
  + _etcd-server-ssl._tcp.<cluster_name>.<base_domain>
    + SRV DNS record for etcd server on that machine with priority 0, weight 10 and port 2380. 
    + Example:
      ```
      # _service._proto.name.                            TTL    class SRV priority weight port target.
      _etcd-server-ssl._tcp.<cluster_name>.<base_domain>  86400 IN    SRV 0        10     2380 etcd-0.<cluster_name>.<base_domain>.
      _etcd-server-ssl._tcp.<cluster_name>.<base_domain>  86400 IN    SRV 0        10     2380 etcd-1.<cluster_name>.<base_domain>.
      _etcd-server-ssl._tcp.<cluster_name>.<base_domain>  86400 IN    SRV 0        10     2380 etcd-2.<cluster_name>.<base_domain>.
      ```

###### Load Balancers Required: [7]
  1. api.<cluster_name>.<base_domain>  port **6443**
     1. Targets Control Plane and Bootstrap nodes
     2. Must be accessable external and internal
  2. api-int.<cluster_name>.<base_domain>  port **22623**
     1. Targets Control Plane and Bootstrap nodes
     2. Must be accessable internal
  3. *.apps.<cluster_name>.<base_domain>  port **443**
     1. Targets nodes hosting Ingress router pods (computer/worker by default)
     2. Must be accessable external and internal
  4. *.apps.<cluster_name>.<base_domain>  port **80**
     1. Targets nodes hosting Ingress router pods (computer/worker by default)
     2. Must be accessable external and internal

###### SSH Key Required: [8]
You must provide an ssh key to the `openshift-install` config file.  This key is required to ssh into the nodes with the `core` user.

### Installation Process:
After the above environment requirements have been met, the installation process consists of the following steps:
  1. Create install-confg file [9] (Will need openshift-install plus pull secret from cloud.redhat.com).  Note these configs are only good for 24h.
  2. Create ignition files [10]
  3. Update ignition files for static ip addresses [11]
  4. Create servers [12]

###### Modify Ignition files
There are a number of methods on how to modify the ignition files.  As described in [11] you can convert the contents of the file you want to add to base64 and directly add the contents to the ignition file.

An easier method is to use File Transpiler [13] to add the contents to the ignition file.  This process uses a specified root file path and added all the files found to the ignition.  This is the process used in [14].

You will need to create a separate ignition file for each host.  The ignition will need to add the ifcfg file for the network interface and the `/etc/hostname` file to set the hostname of the node.  Normally both these details are provided by DHCP.

Additional helpful information is available at [15] and [16].


### Resources:
[0] OCP Architecture Overview: (https://docs.openshift.com/container-platform/4.1/architecture/architecture-rhcos.html)   
[1] OCP Installation Process: (https://docs.openshift.com/container-platform/4.1/architecture/architecture-installation.html)   
[2] OCP Control Plane: (https://docs.openshift.com/container-platform/4.1/architecture/control-plane.html)   
[3] CoreOS + Ignition: (https://docs.openshift.com/container-platform/4.1/architecture/architecture-rhcos.html)   

[4] OCP vSphere Installation: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html)   
[5] Machine requirements: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#minimum-resource-requirements_installing-vsphere)   
[6] DNS: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#installation-dns-user-infra_installing-vsphere)   
[7] Load Balancer: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#installation-network-user-infra_installing-vsphere)   
[8] SSH Key: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#ssh-agent-using_installing-vsphere)   

[9] Install-Config: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#installation-initializing-manual_installing-vsphere)   
[10] Ignition Configs: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#installation-generate-ignition-configs_installing-vsphere)   
[11] Modify Ignition: (https://access.redhat.com/solutions/4175151)   
[11b] Ignition spec: (https://coreos.com/ignition/docs/latest/configuration-v2_3.html)   
[12] Create Cluster: (https://docs.openshift.com/container-platform/4.1/installing/installing_vsphere/installing-vsphere.html#installation-installing-bare-metal_installing-vsphere)   

[13] File Transpiler: (https://github.com/ashcrow/filetranspiler)   
[14] Static IP Lab: (https://github.com/brian-jarvis/ocp-static-lab)

[15] OCP Master Knowledgebase List: (https://access.redhat.com/articles/4217411)   
[16] UPI details/explanation and some helpful "where things go wrong": (https://access.redhat.com/articles/4292081)