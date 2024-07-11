# NeSI analysis environment OODRStudio app

Open OnDemand RStudio app for the NeSI Analysis environment.

## Docker commands

Docker login
```
echo $PAT | docker login ghcr.io -u USERNAME --password-stdin
```

Build and Tag
```
docker build --no-cache -t ghcr.io/lbrick/nesi-ood-rstudio-k8s-app/nesi-ood-rstudio-k8s-app:v0.1 .
```

Push
```
docker push ghcr.io/lbrick/nesi-ood-rstudio-k8s-app/nesi-ood-rstudio-k8s-app:v0.1
```
