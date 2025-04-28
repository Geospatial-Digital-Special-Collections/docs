## Geospatial Digital Special Collections - Installation Notes  

An alternative to using docker desktop.  
- [Mac install](#mac)  
- [Fedora install](#fedora)  

NOTE: no PC version yet ...

### <a name="mac"></a>Command line alternative docker/k8s install for mac:  

- install go (see https://go.dev/doc/install)  

- install docker with homebrew  
  ```
  brew update and brew cleanup
  brew install docker
  brew install docker-compose
  ```

- install k8s (kubectl and kind)  
  kubectl - see (docs)[https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/]  
  kind - see (docs)[https://kind.sigs.k8s.io/docs/user/quick-start/]  

- install colima (for running docker-engine on mac in vm)  
  ```
  brew install colima
  ```

- install kind cloud provider (for external IPs on load balancers)  
  ```
  go install sigs.k8s.io/cloud-provider-kind@latest
  sudo install ~/go/bin/cloud-provider-kind /usr/local/bin
  ```

- Start docker, create k8s cluster, create docker container for kind k8s cache (in gdsc/kubernetes/)
  ```
  colima start --cpu 4 --memory 4
  sed s+{{pwd}}+$(pwd)+g kind-config.yaml | kind create cluster --config -
  docker run -d --name proxy-docker-hub --restart=always \
    --net=kind \
    -e REGISTRY_PROXY_REMOTEURL=https://registry-1.docker.io \
    registry:2  
  kubectl label node kind-control-plane node.kubernetes.io/exclude-from-external-load-balancers
  ```  
  NOTE: on colima memory must be 4 for SOLR, the number of cpus can change.

- bring down cluster and stop docker  
  ```
  kind delete cluster
  colima stop
  ``` 

### <a name="fedora"></a>Command line alternative docker/k8s install for fedora:  

- install go (see https://go.dev/doc/install)  

- install docker engine and tools  
  ```
  sudo dnf-3 config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
  sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  sudo groupadd docker
  sudo usermod -aG docker $USER
  ```
  NOTE: logout and login to effect usermod change  

- install k8s (kubectl, kind, and cloud-provider-kind)  
  kubectl - see (docs)[https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/]  
  ```
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  ```
  kind - see (docs)[https://kind.sigs.k8s.io/docs/user/quick-start/]  
  ```
  curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.26.0/kind-linux-amd64
  chmod +x ./kind
  sudo mv ./kind /usr/local/bin/kind
  ```
  cloud-provider-kind  
  ```
  go install sigs.k8s.io/cloud-provider-kind@latest
  sudo install ~/go/bin/cloud-provider-kind /usr/local/bin
  ```

- Start docker and create k8s cluster (in gdsc/kubernetes/)  
  ```
  systemctl start docker
  sed s+{{pwd}}+$(pwd)+g kind-config.yaml | kind create cluster --config -
  docker run -d --name proxy-docker-hub --restart=always \
    --net=kind \
    -e REGISTRY_PROXY_REMOTEURL=https://registry-1.docker.io \
    registry:2  
  kubectl label node kind-control-plane node.kubernetes.io/exclude-from-external-load-balancers
  ```
  On another shell  
  ```
  sudo cloud-provider-kind
  ```

- Delete k8s cluster and stop docker
  ```
  kind delete cluster
  systemctl stop docker
  ```

- Permissions notes (in gdsc/kubernetes/_localsata)
  ```
  sudo groupadd -g 8983 solr
  sudo usermod -aG solr <username>
  sudo chown -R <username>:solr solr
  sudo chown -R root:root data
  ```

### notes for virtualbox environment  

- add user to vboxsf group on virtualbox to see shared drive
  ```
  sudo usermod -aG vboxsf $USER
  ```
  NOTE: must recreate gdsc folder in virtual box and you can mount the \_localdata/data/ as a shared drive, but not the others.  
  - solr requires a group on host machine to match group on solr container  
  - solr cannot read shared /data folder in container ....  