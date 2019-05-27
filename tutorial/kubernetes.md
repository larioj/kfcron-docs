# Running kfcron on Kubernets

## Application Changes
In order to run kfcron on kubernetes we need to change the configuration so that
the token and the schedule are taken from two independent paths. This way the
configuration can be attached to a gitRepo volume, and the token can be attached
to a secret volume. This is accomplished by this 
[commit](https://github.com/larioj/kfcron/commit/f9bea69).

Now we can run kfcron by doing:
    $ kfcron [path-to-schedule] [path-to-token]

## Dockerizing the application
We need a container wih kfcron in order to run it in a kubernetes cluster. We
created the docker file in this [commit](https://github.com/larioj/kfcron/commit/25ab51b). Now we must build and
pulblish it to docker hub.

### Build and Publish the Container
    # requires: cwd is top level git dir
    
    # build docker container at current sha
    $ docker build -t kfcron:$(git rev-parse --short HEAD) .

    # tag and publish
    $ docker tag kfcron:$(git rev-parse --short HEAD) \
        larioj/kfcron:$(git rev-parse --short HEAD)
    $ docker push larioj/kfcron:$(git rev-parse --short HEAD)

## Create a kubernetes cluseter on gke

### Install GCP SDK
Install instructions at [Google Cloud Docs](https://cloud.google.com/sdk/docs)

### Log In And Create Project
    $ gcloud init

### Enable Kubernetes
https://console.cloud.google.com/apis/api/container.googleapis.com/overview?project=larioj

### Use CLI to create cluster
    $ gcloud container clusters create default --zone us-west1-b
    $ gcloud container clusters resize default --size=1

### Install kubectl
    $ gcloud components install kubectl

## Launch kfcron on kubernetes
Now that we have a running cluster, we can launch kfcron on it. We are going to
create a replica set to run kfcron with the number of replicas set to 1. This
way if the pod fails it will be rescheduled. We will also add some other fancy
features that will make it easier to update the configuration. We will be
mounting a git repo volume that automatically checks out any new changes to
the configuration repository. This is accomplished by the sidecar container
`git-sync`. We will also store the token in a kubernetes secret.

### Create Replica Set Spec
See this [commit](https://github.com/larioj/kfcron/commit/24371e9).

### Create the secret
    $ mkdir -p $HOME/.config/kfcron
    $ touch $HOME/.config/kfcron/kfcron-token.txt
    # add token to file
    $ kubectl create secret generic kfcron-token \
        --from-file=token.txt=$HOME/.config/kfcron/kfcron-token.txt

### Create ReplicaSet
    $ kubectl create -f k8s/kfcron.yaml

## Rejoice
Now whenever we make changes to 
[larioj/kfcron-work-schedule](https://github.com/larioj/kfcron-work-schedule) 
the changes will be picked up by the pods running on kubernetes.
