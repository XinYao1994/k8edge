
<font face="Cambria">

## Pre-requirements
- k8s
  - Version 1.16+
- kubeedge
  - Version 1.15+
  - The installation of kubeedge needs at least two VMs (IP1, IP2)

We build a local k8s cloud on IP1, and IP2 is regarded as an edge device (we assume to use the root account by default):
```
# 1. here, we replace <version> with 1.16.0-00
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add - && \
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list && \
sudo apt-get update -q && \
sudo apt-get install -qy kubelet=<version> kubectl=<version> kubeadm=<version>

# 2. init
kubeadm init

# 3. Do as what the outputs of step 2 require to do.

# 4. download keadm https://github.com/kubeedge/kubeedge/releases/tag/v1.6.1 and put keadm binary in /usr/local/bin

# 5. init the cloudcore and check log. WARN: if your are not using root account, please use 'help' to learn how to specify the configuration path
keadm init --advertise-address="IP1"

# 6. get token
keadm gettoken

# 7. go to IP2, repeat step 1, but we ONLY install kubectl this time

# 8. repeat step 4

# 9. join edgecore 
keadm join --cloudcore-ipport=IP1:10000 --token=token

# 10. complete the steps of "Enable kubectl logs Feature" in https://kubeedge.io/en/docs/setup/keadm/

# 11. tests on two VMs
keam debug check all

```
To install Sedna, follow the step [here](https://github.com/kubeedge/sedna/blob/main/docs/setup/install.md).


</font>

