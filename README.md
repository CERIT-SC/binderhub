# CERIT-SC binderhub

## Description
Binderhub helm chart using Kaniko instead of repo2docker to build images.

## Getting started
### Steps to run with DockerHub registry
1. Clone repository
2. Install helm chart
   <code>helm install \<NAME\> \<PATH/TO/CLONED/REPO\> --namespace=\<NAMESPACE\> -f config.yaml</code>
3. Copy **EXTERNAL-IP** of _proxy-public_ service (<code>kubectl get svc</code>) into the _config.yaml_ file
4. Specify image prefix with your DockerHub name and desired prefix (something like "_dockerhub_user_123/test-img-_")
5. Upgrade helm chart <code>helm upgrade \<NAME\> \<PATH/TO/CLONED/REPO\> --namespace=\<NAMESPACE\> -f config.yaml</code>
6. Create secret containing authentization to DockerHub registry \
  <code>kubectl create secret docker-registry dockercred \
    &emsp;&emsp;&emsp;--docker-server=https://index.docker.io/v1/
    &emsp;&emsp;&emsp;--docker-username=\<USERNAME\>
    &emsp;&emsp;&emsp;--docker-password=\<PASSWORD\>
    &emsp;&emsp;&emsp;--docker-email=\<EMAIL\></code> \
    _Note that dockercred secret name can be changed. In that case it is also necessary to change config.BinderHub.push_secret in config.yaml_
7. Open binder in your browser - accessible on **EXTERNAL-IP** of __binder__ service (<code>kubectl get svc</code>)
8. Run repo
    
## Project status
In development... No main usable version released yet
