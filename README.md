# libresh-staging
Staging cluster for libre.sh project

# instructions to start the cluster

Install [hetzner-kube](https://github.com/xetys/hetzner-kube#how-to-install)
Create a project in Hetzner cloud, and get the token.


```
hetzner-kube context add libresh-staging
hetzner-kube ssh-key add -n sally
hetzner-kube cluster create --ssh-key sally --ha-enabled --worker-count 3 --name libresh-staging
hetzner-kube cluster addon install  --name libresh-staging helm
hetzner-kube cluster addon install  --name libresh-staging ingress
hetzner-kube cluster addon install  --name libresh-staging rook

# get one of master IP
hetzner-kube cluster list
ssh root@78.47.230.104


# install rocker.chat
wget https://raw.githubusercontent.com/kubernetes/charts/master/stable/rocketchat/values.yaml
vi values
# host: chat.libre.sh
# storage-class: "rook-block"
# ingress.enabled: true
helm upgrade --install -f values.yaml awesomechat stable/rocketchat

# watch the pod status
kubectl get po -w
kubectl logs awesomechat-rocketchat-7cc7cc7f45-xhqmq -f

# edit your /etc/hosts to point chat.libre.sh to one of the worker IP 
```

And profit!
