##### ConfigMaps are most preferred way of Putting Environmental Variables.

##### Step#1: Prepare a YML to configure ConfigMap

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-env
    data:
      CONFIGMAP_VARIABLE1: test1
      CONFIGMAP_VARIABLE2: test2
      CONFIGMAP_VARIABLE3: test3
      CONFIGMAP_VARIABLE4: test4

##### Step#2: Refer [SpringBoot Application](https://github.com/rahulvaish/Docker-Java/tree/SpringBootKubernetesConfigMaps). This project on K8 deployment will pick the values from the ConfigMap.

##### Step#3: Deploying SpringBoot Application.

    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: appdeploymentusingcm
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: appdeploymentusingcm
        spec:
          containers:
          - name: appdeploymentusingcm
            image: rahulvaish/springbootkubernetesconfigmaps
            ports:
            - containerPort: 8080
            envFrom:
            - configMapRef:
                name: my-env

##### Step#4: Exposing SpringBoot Application as a service:


    apiVersion: v1
    kind: Service
    metadata:
      labels:
        app: appdeploymentusingcm
      name: appdeploymentusingcm
      namespace: default
    spec:
      ports:
      - nodePort: 31736
        port: 8080
        protocol: TCP
        targetPort: 8080
      selector:
        app: appdeploymentusingcm
      sessionAffinity: None
      type: NodePort
    status:
      loadBalancer: {}
  
  
  
  
