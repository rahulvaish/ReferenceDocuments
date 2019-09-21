### Secrets are mechanism in Kubernetes, where protected data is kept. Let's follow below steps to understand secrets

##### STEP#1: Get a password for your plain text: 

```
echo -n "my-password" | base64
```
##### STEP#2:  Prepare a YML to create secret

```
vi secret.yml
```

    apiVersion: v1
    kind: Secret
    metadata:
      name: my-secret
    type: Opaque
    data:
      ROOT_PASSWORD: bXktcGFzc3dvcmQ=


```
kubectl apply -f secret.yml
```

##### STEP#3: Refer [this example](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingKubernetes/%5B16%5D%20Kubernetes-MySQL.MD) to understand how secrets can be associated with deployments.
