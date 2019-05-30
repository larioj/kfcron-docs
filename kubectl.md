# Kubectl
    : vsplit

## Nodes
    $ kubectl describe node gke-default-default-382176de-965s
    $ kubectl get node 

## Pods
    $ kubectl get pod
    $ kubectl delete pods -l app=kfcron
    $ kubectl delete pods -l app=busybox
    $ kubectl describe pod -l app=kfcron
    $ kubectl describe pod -l app=busybox
    $ kubectl exec -it busybox-85f78cdf8c-8t7rr -- /bin/sh

## Replica Set
    $ kubectl get rs
    $ kubectl describe rs kfcron
    $ kubectl create -f k8s/kfcron.yaml
    $ kubectl apply -f k8s/kfcron.yaml
    $ kubectl edit rs kfcron

## Deployments
    $ kubectl create deployment --image=busybox
    $ kubectl edit deployment busybox
