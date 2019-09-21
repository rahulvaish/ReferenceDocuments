##### Secrets are mechanism in Kubernetes, where protected data is kept. Let's follow below steps to understand secrets

##### STEP#1: Get a password for your plain text: 
```
echo -n "my-super-secret-password" | base64
```
</br>
##### STEP#2:  Prepare a YML to create secret


    apiVersion: v1
    kind: Secret
    metadata:
      name: mysql-secrets
    type: Opaque
    data:
      ROOT_PASSWORD: c3VwZXItc2VjcmV0LXBhc3N3b3JkCg==

