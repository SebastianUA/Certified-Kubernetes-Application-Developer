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

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Understand multi-container Pod design patterns (e.g. sidecar, init and others)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Utilize persistent and ephemeral volumes

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None


## Application Deployment- 20%

### Use Kubernetes primitives to implement common deployment strategies (e.g. blue/green or canary)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Understand Deployments and how to perform rolling updates

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Use the Helm package manager to deploy existing packages

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Kustomize

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None


## Application Observability and Maintenance - 15%

### Understand API deprecations

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Implement probes and health checks

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Use built-in CLI tools to monitor Kubernetes applications

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

### Utilize container logs

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Debugging in Kubernetes

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None


## Application Environment, Configuration and Security - 25%

### Discover and use resources that extend Kubernetes (CRD, Operators)

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Understand authentication, authorization and admission control

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Understand requests, limits, quotas

Examples:
- <details><summary>Example_1: Setting up resources requests for POD:</summary>

</details>

- <details><summary>Example_2: Setting up resources limits for POD:</summary>

</details>

- <details><summary>Example_3: Setting up resources quotas for POD:</summary>

</details>

**Useful official documentation**

- [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)

**Useful non-official documentation**

- None

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

Let us try to understand the whole concept in following points :
- **Understand Resource Limits**: Before we delve into the details of limiting Kubernetes resources, it's crucial to have a clear understanding of what resource limits are and why they are important. Resource limits define the maximum amount of CPU and memory that a container can consume within a Kubernetes cluster. By setting resource limits, you prevent containers from monopolizing resources, ensuring fair allocation and preventing resource exhaustion. When setting resource limits, it's essential to consider the specific requirements of your applications and the available resources in your cluster. By understanding the resource needs of your workloads, you can effectively allocate resources and avoid bottlenecks or performance issues.
- **Set Resource Requests**: In addition to setting resource limits, it's also crucial to define resource requests for your containers. Resource requests specify the minimum amount of CPU and memory that a container requires to run. Kubernetes uses these requests to schedule containers onto appropriate nodes with sufficient resources. By setting resource requests accurately, you ensure that your containers have the necessary resources to function properly. This helps Kubernetes make informed scheduling decisions and prevents overcommitment of resources.
- **Utilize Resource Quotas**: To further enforce resource limits and prevent resource abuse, Kubernetes provides resource quotas. Resource quotas allow you to define limits on the total amount of CPU and memory that can be consumed by all containers within a namespace. By utilizing resource quotas, you can ensure that different teams or projects within your cluster have a fair share of resources. This helps prevent resource contention and allows for better resource management and isolation.
- **Monitor Resource Usage**: Monitoring resource usage is crucial to identify potential bottlenecks, optimize resource allocation, and ensure efficient utilization of your Kubernetes cluster. Kubernetes provides various tools and metrics for monitoring resource usage. By regularly monitoring resource usage, you can identify containers or pods that are consuming excessive resources and take appropriate actions to optimize their resource allocation. This helps maintain the overall health and performance of your cluster.
- **Implement Resource Limits in Pods**: One of the fundamental ways to limit resources in Kubernetes is by setting resource limits directly in the pod specifications. By specifying resource limits at the pod level, you can control the resource consumption of all containers within the pod. When defining resource limits in pods, it's important to consider the resource requirements of individual containers and ensure that the sum of their limits does not exceed the available resources on the node. This prevents resource contention and ensures optimal resource allocation.
- **Use Resource Limits in Namespaces**: In addition to setting resource limits at the pod level, Kubernetes allows you to define resource limits at the namespace level. By setting resource limits at the namespace level, you can enforce resource quotas and prevent resource abuse across multiple pods and containers within the namespace. Using resource limits in namespaces provides a higher level of control and allows for better resource management and isolation. It enables you to allocate resources based on different teams, projects, or applications, ensuring fair resource distribution.
- **Consider Resource Limits for Nodes**: While setting resource limits at the pod and namespace levels is crucial, it's also important to consider resource limits for individual nodes in your Kubernetes cluster. By setting resource limits for nodes, you can prevent resource exhaustion and ensure that no single node becomes overwhelmed with resource-intensive workloads. Considering resource limits for nodes involves monitoring the available resources on each node and setting appropriate limits to prevent overcommitment. This helps maintain the stability and performance of your cluster.
- **Optimize Resource Allocation**: Optimizing resource allocation is an ongoing process that involves continuously evaluating and adjusting resource limits based on the changing needs of your workloads. By regularly analyzing resource usage patterns and performance metrics, you can identify opportunities to optimize resource allocation. Optimizing resource allocation may involve redistributing resources among pods, adjusting resource limits based on workload demands, or utilizing Kubernetes features like horizontal pod autoscaling to automatically scale resources based on predefined metrics.
- **Scale Resources as Needed**: Scaling resources is an essential aspect of managing Kubernetes clusters effectively. By scaling resources, you can ensure that your cluster can handle increased workload demands without compromising performance or stability. Scaling resources can be achieved through various techniques, such as horizontal pod autoscaling, vertical pod autoscaling, or cluster autoscaling. These techniques allow you to dynamically adjust resource allocation based on workload demands, ensuring optimal resource utilization.
- **Regularly Review Resource Usage**: Lastly, it's crucial to regularly review resource usage and make informed decisions based on the insights gained. By periodically analyzing resource usage patterns, you can identify areas for improvement, optimize resource allocation, and prevent resource waste. Regularly reviewing resource usage also helps you stay proactive in managing resource limits, ensuring that your cluster remains efficient and responsive to changing workload demands.


Limit Kubernetes resources with these top tools:
- **ResourceQuota**: ResourceQuota is a Kubernetes feature that allows you to set limits on the amount of resources that can be consumed by a namespace or a group of namespaces. With ResourceQuota, you can define limits for CPU, memory, storage, and other resources, preventing any single application or user from monopolizing resources and impacting the performance of other applications. By setting ResourceQuota, you can ensure fair resource allocation and avoid resource contention issues. This helps in maximizing the efficiency of your Kubernetes cluster by preventing resource wastage and ensuring that resources are distributed evenly among applications.
- **LimitRange**: LimitRange is another useful tool in Kubernetes that allows you to define default resource limits for containers within a namespace. With LimitRange, you can set constraints on CPU, memory, and storage for individual containers, ensuring that they do not exceed the specified limits. By using LimitRange, you can prevent containers from consuming excessive resources, which can lead to performance degradation and resource contention. This tool helps in optimizing resource allocation by enforcing resource limits at the container level, ensuring that each container gets its fair share of resources.
- **HorizontalPodAutoscaler**: The HorizontalPodAutoscaler (HPA) is a Kubernetes feature that automatically scales the number of pods in a deployment based on CPU utilization or custom metrics. With HPA, you can define target CPU utilization thresholds, and the tool will automatically adjust the number of pods to meet the desired utilization level. By leveraging HPA, you can ensure that your applications always have the necessary resources to handle varying workloads. The tool dynamically scales the number of pods up or down, based on the observed CPU utilization, allowing your applications to scale seamlessly and efficiently.
- **PodDisruptionBudget**: PodDisruptionBudget (PDB) is a Kubernetes feature that allows you to define policies for the disruption of pods during maintenance or other events. With PDB, you can specify the minimum number of pods that must be available at any given time, ensuring that your applications remain highly available even during disruptions. By setting PDB, you can prevent unnecessary disruptions to your applications and avoid resource wastage. The tool helps in maximizing efficiency by ensuring that the required number of pods are always available, minimizing any potential impact on your applications.
- **PodPriority**: PodPriority is a Kubernetes feature that allows you to assign priority levels to your pods. With PodPriority, you can specify the importance of different pods, and the Kubernetes scheduler will prioritize the scheduling of higher priority pods over lower priority ones. By leveraging PodPriority, you can ensure that critical pods receive the necessary resources and are scheduled first, maximizing the efficiency of your cluster. This tool helps in optimizing resource allocation by giving priority to important workloads, ensuring that they are not starved of resources.
- **PodAffinity and PodAntiAffinity**: PodAffinity and PodAntiAffinity are Kubernetes features that allow you to influence the scheduling of pods based on their affinity or anti-affinity with other pods. With PodAffinity, you can specify that a pod should be scheduled on the same node as another pod, while PodAntiAffinity allows you to specify that a pod should not be scheduled on the same node as another pod. By using PodAffinity and PodAntiAffinity, you can optimize resource allocation by co-locating or spreading out pods based on their requirements and dependencies. This helps in maximizing efficiency by minimizing network latency and improving resource utilization.
- **NodeAffinity**: NodeAffinity is a Kubernetes feature that allows you to influence the scheduling of pods based on the affinity or anti-affinity with specific nodes in the cluster. With NodeAffinity, you can specify that a pod should be scheduled on nodes that satisfy certain criteria, such as having specific labels or taints. By leveraging NodeAffinity, you can optimize resource allocation by ensuring that pods are scheduled on nodes that meet their specific requirements. This helps in maximizing efficiency by distributing pods across the cluster in a way that minimizes resource contention and improves overall performance.
- **Taints and Tolerations**: Taints and Tolerations are Kubernetes features that allow you to mark nodes with specific taints and specify which pods can tolerate those taints. With taints, you can mark nodes to indicate that they have certain limitations or restrictions, while tolerations allow pods to specify that they can tolerate those taints. By using taints and tolerations, you can optimize resource allocation by ensuring that pods are scheduled on nodes that meet their specific requirements. This helps in maximizing efficiency by preventing pods from being scheduled on nodes that are not suitable for their workload, improving overall performance and resource utilization.
- **PodSecurityPolicy**: PodSecurityPolicy is a Kubernetes feature that allows you to define security policies for pods. With PodSecurityPolicy, you can specify the security context for pods, including settings such as privilege escalation, volume permissions, and container capabilities. By leveraging PodSecurityPolicy, you can ensure that pods adhere to the specified security policies, minimizing the risk of security vulnerabilities. This tool helps in maximizing efficiency by enforcing security best practices and preventing unauthorized access or malicious activities within your Kubernetes cluster.

**Note**: By default, Kubernetes assume that a pod or container within a pod requires `0.5` CPU and `256Mi` of memory.

Examples:
  The examples will be in another section of this cource. This is only understanding of resource requirements.


**Useful official documentation**

- [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)

**Useful non-official documentation**

- [Understanding Resource Requirements — Kubernetes](https://ngugijoan.medium.com/understanding-resource-requirements-kubernetes-9bd34245f779)

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

- <details><summary>Example_5: Encripting secret data at REST:</summary>

  Creating folder for this task:
  ```
  mkdir -p /etc/kubernetes/enc
  ```

  Encrypt secret phrase, for example:
  ```
  echo -n Secret-ETCD-Encryption | base64
    U2VjcmV0LUVUQ0QtRW5jcnlwdGlvbg==
  ```

  Create EncryptionConfiguration `/etc/kubernetes/enc/encryption.yaml` file:
  ```
  ---
  apiVersion: apiserver.config.k8s.io/v1
  kind: EncryptionConfiguration
  resources:
  - resources:
    - secrets
    providers:
    - aesgcm:
      keys:
      - name: key1
        secret: U2VjcmV0LUVUQ0QtRW5jcnlwdGlvbg==
    - identity: {}
  ```

  Open `/etc/kubernetes/manifests/kube-apiserver.yaml` file and put `encryption-provider-config` parameter. Also add volume and volumeMount, for example:
  ```
  spec:
    containers:
    - command:
      - kube-apiserver
    ...
      - --encryption-provider-config=/etc/kubernetes/enc/encryption.yaml
    ...
      volumeMounts:
      - mountPath: /etc/kubernetes/enc
      name: enc
      readOnly: true
    ...
    hostNetwork: true
    priorityClassName: system-cluster-critical
    volumes:
    - hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
      name: enc
    ...
  ```
  Wait till apiserver was restarted:
  ```
  watch crictl ps
  ```

  When `apiserver` will be re-created, we can encrypt all existing secrets. For example, let's do it fort all secrets in `one1` NS:
  ```
  kubectl -n one1 get secrets -o json | kubectl replace -f -
  ```

  To check you can do for example:
  ```
  ETCDCTL_API=3 etcdctl \
  --cert /etc/kubernetes/pki/apiserver-etcd-client.crt \
  --key /etc/kubernetes/pki/apiserver-etcd-client.key \
  --cacert /etc/kubernetes/pki/etcd/ca.crt \
  get /registry/secrets/one1/s1
  ```

</details>


**Useful official documentation**

- [Configure pod with secret](https://kubernetes.io/docs/concepts/configuration/secret)
- [Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)
- [Encrypting Confidential Data at Rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)

**Useful non-official documentation**

- None


### Understand ServiceAccounts

Examples:
- <details><summary>Example_1: Create service accounts:</summary>

  First of all, let's check SA in `prod` namespace:
  ```
  k get sa -n prod
  ```

  To create a new service account:
  ```
  k create sa -n prod my-custom-sa
  ```

  Create clusterrole/role for our needs:
  ```
  k create clusterrole my-custom-role --verb create,delete --resource deployments
  ```

  Next, let's add some permissions for SA:
  ```
  k -n prod create rolebinding my-custom-rb --clusterrole my-custom-role --serviceaccount prod:my-custom-sa
  ```

  To check, you can use the next command:
  ```
  k auth can-i update deployments --as system:serviceaccount:prod:my-custom-sa -n prod
  k auth can-i list pods --as system:serviceaccount:prod:my-custom-sa -n prod
  ```

</details>

- <details><summary>Example_2: Working with SA: need to find witch SA uses for web3 PODs in qa namespace and fix/add list & delete for pods and deployments.</summary>

  Getting SA in NS:
  ```
  k get sa -n qa
  NAME           SECRETS   AGE
  default        0         4m24s
  my-custom-sa   0         2m30s
  ```

  Need to find which resource attached into that SA, checking:
  ```
  k get rolebindings -n qa -o wide
  NAME           ROLE                AGE     USERS   GROUPS   SERVICEACCOUNTS
  my-custom-rb   Role/list-secrets   5m7s                     qa/my-custom-sa
  smoke          Role/smoke          7m51s   smoke            
  smoke-view     ClusterRole/view    7m45s   smoke
  ```

  Gotcha! It's rolebinding with `my-custom-rb` name. That `rb` uses `list-secrets` role. So, need to edit that role and put proper permissions:
  ```
  k edit -n qa role ist-secrets
  ```

  To check, use:
  ```
  k auth can-i list deployments --as system:serviceaccount:qa:my-custom-sa -n qa
  k auth can-i delete deployments --as system:serviceaccount:qa:my-custom-sa -n qa
  k auth can-i list po --as system:serviceaccount:qa:my-custom-sa -n qa
  k auth can-i delete po --as system:serviceaccount:qa:my-custom-sa -n qa
  ```

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Understand Application Security (SecurityContexts, Capabilities, etc.)

Examples:
- <details><summary>Example_1: Working with SecurityContext:</summary>

  Let's create a new POD with image name `redis` and store it into the file to work with:
  ```
  k run sec-con-pod --image=redis --dry-run=client -o yaml > sec-con-pod.yaml
  ```

  Now, let's add some security context into the POD, for example:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-con-pod
    name: sec-con-pod
  spec:
    containers:
    - image: redis
      name: sec-con-pod
      securityContext:
        runAsUser: 1000
        runAsGroup: 1000
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Another way:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-con-pod
    name: sec-con-pod
  spec:
    securityContext:
      runAsUser: 1000
      runAsGroup: 1000
    containers:
    - image: redis
      name: sec-con-pod
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Create POD with:
  ```
  k apply -f sec-con-pod.yaml
  ```

  Verification:
  ```
  k exec -ti sec-con-pod -- id
  
  uid=1000 gid=1000 groups=1000
  ```

</details>

- <details><summary>Example_2:  Working with Capabilities:</summary>

  Let's create a new POD with image name `redis` and store it into the file to work with:
  ```
  k run apa-pod --image=redis --dry-run=client -o yaml > sec-capa-pod.yaml
  ```

  Now, let's add some security context into the POD, for example:
  ```
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: sec-capa-pod
    name: sec-capa-pod
  spec:
    containers:
    - image: redis
      name: sec-capa-pod
      resources: {}
      securityContext:
        runAsUser: 1000
        runAsGroup: 1000
        capabilities:
          add: ["MAC_ADMIN", "SYS_ADMIN"]
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Create POD with:
  ```
  k apply -f sec-capa-pod.yaml
  ```

  Verification:
  ```
  k exec -ti sec-capa-pod -- id
  
  uid=1000 gid=1000 groups=1000
  ```

  And, can check capabilities:
  ```
  k exec -ti sec-capa-pod -- sh -c "cat /proc/1/status | grep Cap"
  ```

</details>


**Useful official documentation**

- [Security context for pods](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

**Useful non-official documentation**

- [Kubernetes SecurityContext Capabilities](https://medium.com/@frankpromiseedah/kubernetes-securitycontext-capabilities-b343f859c633)
- [Kubernetes SecurityContext with practical examples](https://medium.com/marionete/kubernetes-securitycontext-with-practical-examples-67d890558d11)


## Services and Networking - 20%

### Demonstrate basic understanding of NetworkPolicies

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Provide and troubleshoot access to applications via services

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Use Ingress rules to expose applications

Examples:
- <details><summary>Example_1: TBD:</summary>

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None


# Additional useful material

## Articles

1. [Cheatsheet for Kubernetes](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
2. [Handbook](https://www.cncf.io/certification/candidate-handbook)
3. [Exam Tips](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)


## Books

1. None


## Videos

1. [Kubernetes Certified Application Developer (CKAD) with Tests by Mumshad Mannambeth](https://www.udemy.com/course/certified-kubernetes-application-developer)


## Containers and Kubernetes Security Training

1. [Killer.sh CKS practice exam](https://killer.sh/ckad)
2. [Killer Shell CKAD - Interactive Scenarios for Kubernetes Application Developers](https://killercoda.com/killer-shell-ckad)
3. [Kodekloud playground](https://uklabs.kodekloud.com/courses/labs-certified-kubernetes-application-developer)


# Authors

Created and maintained by [Vitalii Natarov](https://github.com/SebastianUA). An email: [vitaliy.natarov@yahoo.com](vitaliy.natarov@yahoo.com).


# License
Apache 2 Licensed. See [LICENSE](https://github.com/SebastianUA/Certified-Kubernetes-Application-Developer/blob/main/LICENSE) for full details.