# Kubernetes Development
    : vsplit

## Set up

### Install GCP SDK
Install instructions at [Google Cloud Docs](https://cloud.google.com/sdk/docs)

#### Using homebrew
    $ brew cask install google-cloud-sdk

### Log In And Create Project
    $ gcloud init

### Enable Kubernetes
https://console.cloud.google.com/apis/api/container.googleapis.com/overview?project=larioj

### Create Cluster
    $ gcloud container clusters --help
    $ gcloud container clusters create default --zone us-west1-b
    $ gcloud container clusters resize default --size=1
    $ gcloud container clusters list

### Use free tier
    $ gcloud compute machine-types list
    $ gcloud container node-pools create default \
        --cluster=default \
        --machine-type=f1-micro \
        --disk-size=30GB \
        --num-nodes=1
    $ gcloud container node-pools list --cluster=default
    $ gcloud container node-pools delete xxxx --cluster=default

### Install Kubectl
    $ gcloud components install kubectl
    $ gcloud container clusters get-credentials default

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
    $ kubectl create -f k8s/kfcron.yaml
    $ kubectl get rs
    $ kubectl describe rs kfcron

## Update Replaca Set
    $ kubectl delete pods -l app=kfcron
    $ kubectl describe pod -l app=kfcron
    $ kubectl exec -it xxx -- /bin/sh
    $ kubectl apply -f k8s/kfcron.yaml
    $ kubectl edit rs kfcron

## Debug Logs
    $ kubectl logs -l app=kfcron -c git-sync

## Create Token Secret
    $ touch $HOME/kfcron-token.txt
    # add token to file
    $ kubectl create secret generic kfcron-token \
        --from-file=token.txt=$HOME/.config/kfcron/kfcron-token.txt --dry-run -o yaml \
      | kubectl apply -f -
