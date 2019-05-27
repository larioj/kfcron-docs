# Kubernetes Development
  : vsplit

## Set up

### Install GCP SDK
Install instructions at [Google Cloud Docs](https://cloud.google.com/sdk/docs)

### Log In And Create Project
  $ gcloud init

### Enable Kubernetes
https://console.cloud.google.com/apis/api/container.googleapis.com/overview?project=larioj

### Learn how to use it
  $ gcloud --help
  $ gcloud config set compute/zone us-west1-b
  $ gcloud compute --help
  $ gcloud compute disks list
  $ gcloud compute instances list
  $ gcloud compute instances delete default
  $ gcloud container clusters --help
  $ gcloud container clusters create default --zone us-west1-b
  $ gcloud container clusters resize default --size=1
  $ gcloud container clusters list

### Kubectl
  $ gcloud components install kubectl
  $ kubectl get node

## Files
  k8s/kfcron.yaml

## Publish docker image
  $ docker login
  $ docker build -t kfcron:$(git rev-parse --short HEAD) .
  $ docker tag kfcron:$(git rev-parse --short HEAD) \
      larioj/kfcron:$(git rev-parse --short HEAD)
  $ docker images
  $ docker push larioj/kfcron:$(git rev-parse --short HEAD)

## Create Replica Set
  $ kubectl get rs
  $ kubectl describe rs kfcron
  $ kubectl get pod
  $ kubectl delete pods -l app=kfcron
  $ kubectl describe pod -l app=kfcron
  $ kubectl exec -it kfcron-bc89r -- /bin/bash
  $ kubectl create -f k8s/kfcron.yaml
  $ kubectl apply -f k8s/kfcron.yaml

  $ kubectl logs -l app=kfcron -c git-sync

## Create Token Secret
  $ touch $HOME/kfcron-token.txt
  # add token to file
  $ kubectl create secret generic kfcron-token \
      --from-file=token.txt=$HOME/.config/kfcron/kfcron-token.txt --dry-run -o yaml \
    | kubectl apply -f -
