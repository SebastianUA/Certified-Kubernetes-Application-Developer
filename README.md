# Certified Kubernetes Application Developer (CKAD) in 2024

Certified Kubernetes Application Developer (CKAD): Open new career doors – prove your Kubernetes & cloud native developer skills with the CKAD certification that is recognized globally, vendor-neutral, and relevant across all industries.

<p align="center">
  <img width="360" src="ckad.png">
</p>

# Certification

- Duration of Exam: **120 minutes**
- Number of questions: **15-20 hands-on performance-based tasks**
- Passing score: **66%**
- Certification validity: **2 years**
- Cost: **$395 USD**
- Exam Eligibility: **12 Month**, with a free retake within this year.
- Software Version: **Kubernetes v1.29**
- [The official website with certification](https://training.linuxfoundation.org/certification/certified-kubernetes-application-developer-ckad)
- [CNCF Exam Curriculum repository](https://github.com/cncf/curriculum/)
- [Tips & Important Instructions: CKS](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
- [Candidate Handbook](https://www.cncf.io/certification/candidate-handbook)
- [Verify Certification](https://training.linuxfoundation.org/certification/verify/)


# Structure of certification

## Application Design and Build - 20%

### Define, build and modify container images

Examples:
- <details><summary>Example_1: Build and modify container images with Docker:</summary>

  An example of Dockerfile:
  ```
  FROM ubuntu
  MAINTAINER Vitalii <vitaliy.natarov@yahoo.com>
  
  RUN \
    add-apt-repository -y ppa:chris-lea/redis-server && \
    apt-get update && \
    apt-get -y install \
            redis-server && \
    rm -rf /var/lib/apt/lists/*
  
  RUN rm /usr/sbin/policy-rc.d
  CMD ["/usr/bin/redis-server"]
  
  EXPOSE 6379
  ```

  To build a new image, you can use:
  ```
  docker build Dockerfile -t my-custom-image
  ```

  If you want to add environment variables:
  ```
  docker run -e ENV=dev my-custom-image
  ```

  To push image, use:
  ```
  docker push my-custom-image
  ```

</details>

- <details><summary>Example_2: Build and modify container images with Podman:</summary>

  An example of Dockerfile:
  ```
  FROM ubuntu
  MAINTAINER Vitalii <vitaliy.natarov@yahoo.com>
  
  RUN \
    add-apt-repository -y ppa:chris-lea/redis-server && \
    apt-get update && \
    apt-get -y install \
            redis-server && \
    rm -rf /var/lib/apt/lists/*
  
  RUN rm /usr/sbin/policy-rc.d
  CMD ["/usr/bin/redis-server"]
  
  EXPOSE 6379
  ```

  To build a new image, you can use:
  ```
  podman build Dockerfile -t my-custom-image
  ```

  If you want to add environment variables:
  ```
  podman run -e ENV=dev my-custom-image
  ```

  To push image, use:
  ```
  podman push my-custom-image
  ```

</details>


**Useful official documentation**

- None

**Useful non-official documentation**

- [Docker documentation](https://docs.docker.com/build)
- [Podman documentation](https://docs.podman.io/en/latest/Introduction.html)


### Choose and use the right workload resource (Deployment, DaemonSet, CronJob, etc.)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Understand multi-container Pod design patterns (e.g. sidecar, init and others)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Utilize persistent and ephemeral volumes

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>


## Application Deployment- 20%

### Use Kubernetes primitives to implement common deployment strategies (e.g. blue/green or canary)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Understand Deployments and how to perform rolling updates

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Use the Helm package manager to deploy existing packages

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Kustomize

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

## Application Observability and Maintenance - 15%

### Understand API deprecations

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Implement probes and health checks

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Use built-in CLI tools to monitor Kubernetes applications

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Utilize container logs

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Debugging in Kubernetes

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>


## Application Environment, Configuration and Security - 25%

### Discover and use resources that extend Kubernetes (CRD, Operators)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Understand authentication, authorization and admission control

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Understand requests, limits, quotas

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Understand ConfigMaps

Kubernetes ConfigMap is a built-in Kubernetes API object that’s designed to store your application’s non-sensitive key-value config data. ConfigMaps allow you to keep config values separate from your code and container images. Values can be strings or Base64-encoded binary data.

Once you’ve created a ConfigMap, its content will be saved to your Kubernetes cluster. Pods can then consume the ConfigMap’s values as environment variables, command line arguments, or filesystem volumes.

ConfigMaps are intended to store relatively small amounts of simple data. The total size of a ConfigMap object must be less than 1 MiB. If you need to store more data, you should split your configuration into multiple ConfigMaps or consider using a separate database or key-value store.

It’s good practice to use a ConfigMap to supply any configurable values that your app needs at runtime. The key-value pairs within the ConfigMap can be injected directly into your Pods in several different formats. Because ConfigMaps are part of the Kubernetes API, it’s possible to alter their values without re-deploying your app’s Pods.

Examples:
- <details><summary>Example_1: Create a new configMap:</summary>

  1. Let's create a new configmap with `my-cm` name in `prod` namespace:
    ```
    k create cm my-cm -n prod --from-literal=username=username --from-literal=password=MyPasswd --from-literal=host=localhost
    ```

  2. Let's create a new configmap with `my-cm2` name in `prod` namespace:
    ```
    echo "username=user" >> creds.txt
    echo "password=password" >> creds.txt
    echo "host=localhost" >> creds.txt
    
    k create cm my-cm2 -n prod --from-file=creds=creds.txt
    ```

    NOTE: An example, of `creds.txt` file:
    ```
    username=user
    password=password
    host=localhost
    ```

    To get data, you can use:
    ```
    k describe cm -n prod my-cm2
    
    Name:         my-cm2
    Namespace:    prod
    Labels:       <none>
    Annotations:  <none>
    
    Data
    ====
    creds:
    ----
    username=user
    password=password
    host=localhost
    
    BinaryData
    ====
    
    Events:  <none>
    ```
</details>

- <details><summary>Example_2: Consume data from ConfigMap in Pod as environment variable or mount it as volume:</summary>

  In `prod` ns get configmaps and create a new pod with `env-pod-vars` name. The `my-cm` configmap attach as ENV and `my-cm2` another one - as volume. Names, you can use by your choise.
  First of all, gets cms:
  ```
  k get cm -n prod
  NAME               DATA   AGE
  kube-root-ca.crt   1      11m
  my-cm              3      9m30s
  my-cm2             1      5m59s
  ```

  Create Pod:
  ```
  k run env-pod-vars -n prod --image=nginx --dry-run=client -o yaml > env-pod-vars.yaml
  ```

  Next, edit `env-pod-vars.yaml` file and create ENV + mount volume:
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: env-pod-vars
    name: env-pod-vars
    namespace: prod
  spec:
    volumes:
    - name: config-volume
      configMap:
        name: my-cm2
    containers:
    - image: nginx
      name: env-pod-vars
      volumeMounts:
      - name: config-volume
        mountPath: /etc
      env:
      - name: password
        valueFrom:
          configMapKeyRef:
            name: my-cm
            key: password
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  
  ```

  Another solution:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: env-pod-vars
    name: env-pod-vars
    namespace: prod
  spec:
    volumes:
    - name: config-volume
      configMap:
        name: my-cm2
    containers:
    - image: nginx
      name: env-pod-vars
      volumeMounts:
      - name: config-volume
        mountPath: /etc
      envFrom:
        - configMapRef:
            name: my-cm
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply the changes:
  ```
  k apply -f env-pod-vars.yaml
  ```

</details>

**Useful official documentation**

- [Configure pod with configmap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

**Useful non-official documentation**

- None


### Define resource requirements

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Create & consume Secrets

A Kubernetes secret is an object storing sensitive pieces of data such as usernames, passwords, tokens, and keys. Secrets are created by the system during an app installation or by users whenever they need to store sensitive information and make it available to a pod.

If passwords, tokens, or keys were simply part of a pod definition or container image, they could be accidentally exposed during Kubernetes operations. Therefore, the most important function of the secret is to prevent accidental exposure of the information stored in it while at the same time making it available wherever the user needs it.

Examples:
- <details><summary>Example_1: Create new secrets:</summary>

  Lets create a new secret with `new-secret` name in `default` namespace:
  ```
  k create secret generic new-secret --from-literal=user=username --from-literal=password=password
  ```

  Getting secrets:
  ```
  k get secret
  NAME         TYPE     DATA   AGE
  new-secret   Opaque   2      17s
  ```

</details>

- <details><summary>Example_2: Consume secret in Pod as environment variable:</summary>

  Let's create a new pod with `sec-env-var` name to play around with secret and environment variable:
  ```
  k run sec-env-var --image=nginx --dry-run=client -o yaml > sec-env-var.yaml
  ```

  Then, open yaml file and attach secret as environemnt variable:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-env-var
    name: sec-env-var
  spec:
    containers:
    - image: nginx
      name: sec-env-var
      env:
      - name: new-secret-username
        valueFrom:
          secretKeyRef:
            name: new-secret
            key: user
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Or, another solution:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-env-var
    name: sec-env-var
  spec:
    containers:
    - image: nginx
      name: sec-env-var
      envFrom:
      - secretRef:
          name: new-secret
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply the yaml:
  ```
  k apply -f sec-env-var.yaml
  ```

</details>

- <details><summary>Example_3: Consume secret in Pod as volume:</summary>

  Let's create a new pod with `sec-volume-var` name to play around with secret and mount it as volume:
  ```
  k run sec-volume-var --image=nginx --dry-run=client -o yaml > sec-volume-var.yaml
  ```

  Then, open yaml file and mount secret as volume. The name you can use by yourself:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-volume-var
    name: sec-volume-var
  spec:
    volumes:
    - name: new-secret
      secret:
        secretName: new-secret
    containers:
    - image: nginx
      name: sec-volume-var
      volumeMounts:
      - name: new-secret
        mountPath: /etc
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Or, another way:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-volume-var
    name: sec-volume-var
  spec:
    volumes:
    - name: new-secret
      secret:
        secretName: new-secret
        items:
        - key: user
          path: username
    containers:
    - image: nginx
      name: sec-volume-var
      volumeMounts:
      - name: new-secret
        mountPath: /etc
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply the yaml:
  ```
  k apply -f sec-volume-var.yaml
  ```

</details>

- <details><summary>Example_4: Get, read, decode secrets:</summary>

  Getting secrets:
  ```
  k get secret
  NAME         TYPE     DATA   AGE
  new-secret   Opaque   2      17s
  ```

  Let's read secret:
  ```
  k get secrets new-secret -o yaml

  apiVersion: v1
  data:
    password: cGFzc3dvcmQ=
    user: dXNlcm5hbWU=
  kind: Secret
  metadata:
    creationTimestamp: "2024-07-29T22:35:30Z"
    name: new-secret
    namespace: default
    resourceVersion: "3384"
    uid: 2b05cae8-5111-48b3-9876-2ce870670404
  type: Opaque
  ```

  Decode it and store secret into file per key, for example:
  ```
  k get secrets new-secret -ojsonpath='{.data.user}' | base64 -d > user.txt
  k get secrets new-secret -ojsonpath='{.data.password}' | base64 -d > password.txt
  ```

</details>

**Useful official documentation**

- [Configure pod with secret](https://kubernetes.io/docs/concepts/configuration/secret)
- [Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

**Useful non-official documentation**

- None


### Understand ServiceAccounts

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Understand Application Security (SecurityContexts, Capabilities, etc.)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

## Services and Networking - 20%

### Demonstrate basic understanding of NetworkPolicies

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Provide and troubleshoot access to applications via services

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Use Ingress rules to expose applications

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>


# Additional useful material

## Articles

1. [Cheatsheet for Kubernetes](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
2. [Handbook](https://www.cncf.io/certification/candidate-handbook)
3. [Exam Tips](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)


## Books

1. None


## Videos

1. None


## Containers and Kubernetes Security Training

1. [Killer.sh CKS practice exam](https://killer.sh/ckad)
2. [Killer Shell CKAD - Interactive Scenarios for Kubernetes Application Developers](https://killercoda.com/killer-shell-ckad)
3. [Kodekloud playground](https://uklabs.kodekloud.com/courses/labs-certified-kubernetes-application-developer)


# Authors

Created and maintained by [Vitalii Natarov](https://github.com/SebastianUA). An email: [vitaliy.natarov@yahoo.com](vitaliy.natarov@yahoo.com).


# License
Apache 2 Licensed. See [LICENSE](https://github.com/SebastianUA/Certified-Kubernetes-Application-Developer/blob/main/LICENSE) for full details.