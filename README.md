# Certified Kubernetes Application Developer (CKAD) in 2024

Certified Kubernetes Application Developer (CKAD): Open new career doors – prove your Kubernetes & cloud native developer skills with the CKAD certification that is recognized globally, vendor-neutral, and relevant across all industries.

<p align="center">
  <img width="360" src="ckad.png">
</p>

# Certification

- Duration of Exam: **120 minutes**.
- Number of questions: **15-20 hands-on performance-based tasks**.
- Passing score: **66%** or above.
- Certification validity: **2 years**.
- Cost: **$395 USD**.
- Exam Eligibility: **12 Month**, with a free retake within this year.
- Software Version: **Kubernetes v1.30**.
- [The official website with certification](https://training.linuxfoundation.org/certification/certified-kubernetes-application-developer-ckad)
- [CNCF Exam Curriculum repository](https://github.com/cncf/curriculum/)
- [Tips & Important Instructions: CKAD](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
- [Candidate Handbook](https://www.cncf.io/certification/candidate-handbook)
- [Verify Certification](https://training.linuxfoundation.org/certification/verify/)

# Global Tips

## Shortcuts / Aliases

- `po` = Pods
- `rs` = ReplicaSets
- `deploy` = Deployments
- `svc` = Services
- `ns` = Namespaces
- `netpol` = Network Policies
- `pv` = Persistent Volumes
- `pvc` = Persistent Volume Claims
- `sa` = Service Accounts
- `cm` = ConfigMaps
- `cj` = CrobJobs
- `ep` = Endpoints
- `crd` = Customresourcedefinitions

## Get all resources in Kubernetes cluster

To get all resources in all namespaces: 

```shell
kubectl get all --all-namespaces
```

Or, for specific namespace:
```shell
kubectl get all -n my-test-ns
```

## Formatting Output with kubectl

The default output format for all kubectl commands is the human-readable plain-text format. The `-o` flag allows us to output the details in several different formats. An example of command:
```shell
kubectl [command] [TYPE] [NAME] -o <output_format>
```

Here are some of the commonly used formats:
- `-o json` - Output a JSON formatted API object.
- `-o name` - Print only the resource name and nothing else.
- `-o wide` - Output in the plain-text format with any additional information.
- `-o yaml` - Output a YAML formatted API object.

## Kubectl Autocomple and Alias

Configure the Kubectl autocomplete and the `alias k=kubectl`:
```shell
source <(kubectl completion bash)  # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc  # add autocomplete permanently to your bash shell.
```

NOTE: If you use `ZSH` or another shell, modify the correct path to configuration if so.

You can also use a shorthand alias for kubectl that also works with completion:
```shell
alias k=kubectl
complete -F __start_kubectl k
```

## Install Kubernetes cluster in Unix/Linux

First of all, getting `multipass` from official site:
- [Install on Linux](https://multipass.run/install#linux).
- [Install on MacOS](https://multipass.run/download/macos).

In [hands-on/00_Installers](https://github.com/SebastianUA/Certified-Kubernetes-Application-Developer/tree/main/hands-on/00_Installers) folder you can find some simple installers. For now, it's available only for MacOS.


# Structure of certification

## Application Design and Build - 20%

### Define, build and modify container images

Examples:
- <details><summary>Example_1: Build and modify container images with Docker:</summary>

  An example of Dockerfile:
  ```dockerfile
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
  ```shell
  docker build Dockerfile -t my-custom-image
  ```

  If you want to add environment variables:
  ```shell
  docker run -e ENV=dev my-custom-image
  ```

  To push image, use:
  ```shell
  docker push my-custom-image
  ```

</details>

- <details><summary>Example_2: Build and modify container images with Podman:</summary>

  An example of Dockerfile:
  ```dockerfile
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
  ```shell
  podman build Dockerfile -t my-custom-image
  ```

  If you want to add environment variables:
  ```shell
  podman run -e ENV=dev my-custom-image
  ```

  To push image, use:
  ```shell
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

- <details><summary>Example_1: Working with Job:</summary>

  To get jobs, use:
  ```shell
  k get jobs.batch
  ```

  Let's create a new job. Set `my-job-1` name:
  ```shell
  k create job my-job-1 --image=nginx --dry-run=client -o yaml > my-job-1.yaml
  ```

  Open file for edit and let's set `backoffLimit` to `25`. Also, set `parallelism` to `4`:
  ```yaml
  apiVersion: batch/v1
  kind: Job
  metadata:
    creationTimestamp: null
    name: my-job-1
  spec:
    backoffLimit: 25
    parallelism: 4
    template:
      metadata:
        creationTimestamp: null
      spec:
        containers:
        - image: nginx
          name: my-job-1
          resources: {}
        restartPolicy: Never
  status: {}
  ```

  Some usefull parameters:
  - `backoffLimit`: Limits the number of retries before considering the Job as failed.
  - `ttlSecondsAfterFinished`: A way to clean up finished Jobs (either Complete or Failed) automatically is to use a TTL mechanism provided by a TTL controller for finished resources, by specifying the `.spec.ttlSecondsAfterFinished` field of the Job. For example: `ttlSecondsAfterFinished: 100`.
  - `parallelism`: Determines the maximum number of Pods that can run in parallel. For example, if you set `parallelism` to `2`, Kubernetes will ensure that no more than two pods are running at the same time.
  - `completions`: Determines the desired number of successfully finished pods a Job should have. For example, if you set `completions` to `5`, Kubernetes will ensure that five Pods have completed successfully.
  - `activeDeadlineSeconds`: The activeDeadlineSeconds applies to the duration of the job, no matter how many Pods are created. Once a Job reaches activeDeadlineSeconds , all of its running Pods are terminated and the Job status will become type: Failed with reason: DeadlineExceeded."

  To delete job:
  ```shell
  k delete jobs.batch my-job-1
  ```

</details>

- <details><summary>Example_2: Working with Cronjob:</summary>

  To get cronjobs, use:
  ```shell
  k get cronjobs
  ```

  Let's create a new job: 
  ```shell
  k create cronjob my-cronjob-1 --image=nginx --schedule='00 00 * * *' --dry-run=client -o yaml > my-cronjob-1.yaml
  ```

  Set `my-cronjob-1` name. Set `backoffLimit` to `25`. Also, set `parallelism` to `4` and set `completions` to `3`:
  ```yaml
  apiVersion: batch/v1
  kind: CronJob
  metadata:
    creationTimestamp: null
    name: my-cronjob-1
  spec:
    jobTemplate:
      metadata:
        creationTimestamp: null
        name: my-cronjob-1
      spec:
        completions: 3
        parallelism: 3
        backoffLimit: 25
        activeDeadlineSeconds: 20
        startingDeadlineSeconds: 10
        concurrencyPolicy: 3
        template:
          metadata:
            creationTimestamp: null
          spec:
            containers:
            - image: nginx
              name: my-cronjob-1
              resources: {}
            restartPolicy: OnFailure
    schedule: 00 00 * * *
  status: {}
  ```

  Some usefull parameters:
  - `backoffLimit`: Limits the number of retries before considering the Job as failed.
  - `ttlSecondsAfterFinished`: A way to clean up finished Jobs (either Complete or Failed) automatically is to use a TTL mechanism provided by a TTL controller for finished resources, by specifying the `.spec.ttlSecondsAfterFinished` field of the Job. For example: `ttlSecondsAfterFinished: 100`.
  - `parallelism`: Determines the maximum number of Pods that can run in parallel. For example, if you set `parallelism` to `2`, Kubernetes will ensure that no more than two pods are running at the same time.
  - `completions`: Determines the desired number of successfully finished pods a Job should have. For example, if you set `completions` to `5`, Kubernetes will ensure that five Pods have completed successfully.
  - `activeDeadlineSeconds`: The activeDeadlineSeconds applies to the duration of the job, no matter how many Pods are created. Once a Job reaches activeDeadlineSeconds , all of its running Pods are terminated and the Job status will become type: Failed with reason: DeadlineExceeded."
  - `startingDeadlineSeconds` - `startingDeadlineSeconds` determines the deadline in seconds for starting the job if it misses its scheduled time. If a CronJob misses its scheduled time for any reason and more time than `startingDeadlineSeconds` has passed, the job will not be started.
  - `concurrencyPolicy` - field is also optional. It specifies how to treat concurrent executions of a Job that is created by this CronJob. Allow (default): The CronJob allows concurrently running Jobs. Forbid: The CronJob does not allow concurrent runs; if it is time for a new Job run and the previous Job run hasn't finished yet, the CronJob skips the new Job run. Also note that when the previous Job run finishes, .spec.startingDeadlineSeconds is still taken into account and may result in a new Job run. Replace: If it is time for a new Job run and the previous Job run hasn't finished yet, the CronJob replaces the currently running Job run with a new Job run.
  - `successfulJobsHistoryLimit` - This field specifies the number of successful finished jobs to keep. The default value is 3. Setting this field to 0 will not keep any successful jobs.
  - `failedJobsHistoryLimit` - This field specifies the number of failed finished jobs to keep. The default value is 1. Setting this field to 0 will not keep any failed jobs.

  To delete job:
  ```shell
  k delete cronjobs my-cronjob-1
  ```

</details>

- <details><summary>Example_3: Create Job from CronJob:</summary>

  Check if jronjob created already:
  ```shell
  k get cronjob
  ```

  Let's create `job` from `cronjob`, for example:
  ```shell
  k create job my-job-from-cronjob --from=cronjob/my-cronjob-1 --dry-run=client -oyaml > my-job-from-cronjob.yaml
  ```

  If you need to add/change something, you can edit that file.

  To apply, use:
  ```shell
  k apply -f my-job-from-cronjob.yaml
  ```

  Get job:
  ```shell
  k get jobs
  ```

</details>

- <details><summary>Example_4: Working with DaemonSet:</summary>

  To get DaemonSet, for example in `prod` namespace:
  ```shell
  k get deamonset -n prod
  ```

  Edit and update the definition of one or more daemonset:
  ```shell
  k edit ds <deamonset_name>
  ```

  Delete a daemonset:
  ```shell
  k delete deamonset <deamonset_name>
  ```

  Create a new daemonset:
  ```shell
  k create deamonset <deamonset_name>
  ```

  Manage the rollout of a daemonset:
  ```shell
  k rollout daemonset
  ```

  Display the detailed state of daemonsets within a namespace:
  ```shell
  k describe ds <deamonset_name> -n <namespace_name>
  ```

</details>

- <details><summary>Example_5: Working with Deployment:</summary>

  To get Deployment, for example in `prod` namespace:
  ```shell
  k get deployment -n prod
  ```

  Or, if you have labels:
  ```shell
  k get deployment -n prod -l env=dev
  k get deployment -n prod --selector env=dev
  ```

  Display the detailed state of deploy within a namespace:
  ```shell
  k describe deploy <deployment_name> -n <namespace_name>
  ```

  Edit and update the definition of one or more deploy:
  ```shell
  k edit deploy <deployment_name>
  ```

  Delete a deploy:
  ```shell
  k delete deploy <deployment_name> -n <namespace_name>
  ```

  Create one a new deployment:
  ```shell
  k create deploy <deployment_name>
  ```

  See the rollout status of a deployment:
  ```shell
  k rollout status deployment <deployment_name> -n <namespace_name>
  ```

</details>

**Useful official documentation**

- [Kubernetes Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
- [Kubernetes CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

**Useful non-official documentation**

- None

### Understand multi-container Pod design patterns (e.g. sidecar, init and others)

Examples:
- <details><summary>Example_1: Using sidecar container:</summary>

  Sidecar containers are the secondary containers that run along with the main application container within the same Pod. These containers are used to enhance or to extend the functionality of the primary app container by providing additional services, or functionality such as logging, monitoring, security, or data synchronization, without directly altering the primary application code. Typically, you only have one app container in a Pod. For example, if you have a web application that requires a local webserver, the local webserver is a sidecar and the web application itself is the app container.


  An example with sidecar container:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-with-sidecar
  spec:
     # Create a volume called 'shared-logs' that the app and sidecar share.
    volumes:
    - name: shared-logs 
      emptyDir: {}

     # In the sidecar pattern, there is a main application container and a sidecar container.
    containers:
     # Main application container
    - name: app-container
      # Simple application: write the current date to the log file every five seconds
      image: alpine  alpine is a simple Linux OS image
      command: ["/bin/sh"]
      args: ["-c", "while true; do date >> /var/log/app.txt; sleep 5;done"]

       # Mount the pod's shared log file into the app container. The app writes logs here.
      volumeMounts:
      - name: shared-logs
        mountPath: /var/log

     # Sidecar container
    - name: sidecar-container
       # Simple sidecar: display log files using nginx. In reality, this sidecar would be a custom image that uploads logs to a third-party or storage service.
      image: nginx:1.7.9
      ports:
        - containerPort: 80
       # Mount the pod's shared log file into the sidecar container. In this case, nginx will serve the files in this directory.
      volumeMounts:
      - name: shared-logs
        mountPath: /usr/share/nginx/html  nginx-specific mount path
  ```

</details>

- <details><summary>Example_2: Using init container:</summary>

  A Pod can have multiple containers running apps within it, but it can also have one or more init containers, which are run before the app containers are started.

  Init containers are exactly like regular containers, except:
  - Init containers always run to completion.
  - Each init container must complete successfully before the next one starts.
  
  If a Pod's init container fails, the kubelet repeatedly restarts that init container until it succeeds. However, if the Pod has a restartPolicy of Never, and an init container fails during startup of that Pod, Kubernetes treats the overall Pod as failed.

  To specify an init container for a Pod, add the initContainers field into the Pod specification, as an array of container items (similar to the app containers field and its contents). See Container in the API reference for more details.

  An example of init-container:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: myapp-pod
    labels:
      app: myapp
  spec:
    containers:
    - name: myapp-container
      image: busybox:1.28
      command: ['sh', '-c', 'echo The app is running! && sleep 3600']
    initContainers:
    - name: init-myservice
      image: busybox:1.28
      command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
    - name: init-mydb
      image: busybox:1.28
      command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
  ```

</details>

- <details><summary>Example_3: Using adapter container:</summary>

  At its core, the Adapter Container pattern involves deploying an additional container within the same pod as your main application container. This secondary container acts as an intermediary, adapting or modifying data or communication between your main application and external systems. The main goal is to ensure seamless integration without burdening your main application with protocol translation, data format conversion, or encryption.

  The Adapter Container pattern offers several advantages:
  - Modularity: Your main application stays focused on its core functionality while the adapter container takes care of data adaptation and encryption. This separation of concerns enhances code maintainability.
  - Reusability: The adapter container can be reused across multiple pods or services that require similar data transformation or encryption. It promotes code reuse and consistency.
  - Simplicity: Your main application doesn't need to deal with the complexities of data format conversion or TLS encryption. This simplifies the codebase and makes it more manageable.
  - Security: TLS encryption is crucial for secure data transmission. By centralizing encryption within the adapter container, you ensure that data is always encrypted before leaving your application.

  An example with adapter container:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: webserver-1
    labels:
      app: webserver
  spec:
    volumes:
      - name: nginx-conf
        configMap:
          name: nginx-conf
          items:
            - key: default.conf
              path: default.conf
    containers:
      - name: webserver
        image: nginx
        ports:
          - containerPort: 80
        volumeMounts:
          - mountPath: /etc/nginx/conf.d
            name: nginx-conf
            readOnly: true
      - name: adapter
        image: nginx/nginx-prometheus-exporter:0.4.2
        args: ["-nginx.scrape-uri","http://localhost/nginx_status"]
        ports:
          - containerPort: 9113
  ```

</details>

**Useful official documentation**

- [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers)
- [Sidecar Containers](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/)

**Useful non-official documentation**

- [Kubernetes Multi Container Pod patterns - Adapter Container Pattern](https://navyadevops.hashnode.dev/kubernetes-multi-container-pod-patterns-adapter-container-pattern)
- [Kubernetes Sidecar Container - Best Practices and Examples](https://www.atatus.com/blog/kubernetes-sidecar-container-best-practices-and-examples/what-is-a-sidecar-container)

### Utilize persistent and ephemeral volumes

Examples:
- <details><summary>Example_1: Working with storageClass:</summary>

  Get storage classes:
  ```shell
  k get sc
  ```

  Create a new storageClass, for example with `local-storage` name inside `local-storage.yaml` file:
  ```yaml
  apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: local-storage
  provisioner: kubernetes.io/no-provisioner
  volumeBindingMode: WaitForFirstConsumer
  ```

  Apply the file:
  ```shell
  k apply -f local-storage.yaml
  ```

</details>

- <details><summary>Example_2: Working with PV and PVC:</summary>

  To get PV in `qa` namespace:
  ```shell
  k get pv -n qa
  ```

  To get PVC in `qa` namespace:
  ```shell
  k get pvc -n qa
  ```

  Let's add a new PersistentVolume to `pv-log.yaml` file:
  ```yaml
  ---
  apiVersion: v1
  kind: PersistentVolume
  metadata:
    name: pv-log
  spec:
    persistentVolumeReclaimPolicy: Retain
    accessModes:
      - ReadWriteMany
    capacity:
      storage: 100Mi
    hostPath:
      path: /pv/log
  ```

  Apply file:
  ```shell
  K apply -f pv-log.yaml
  ```
  
  Let's add a new PersistentVolumeClaim to `local-pvc.yaml` file:
  ```yaml
  ---
  kind: PersistentVolumeClaim
  apiVersion: v1
  metadata:
    name: local-pvc
  spec:
    accessModes:
    - ReadWriteOnce
    storageClassName: local-storage
    resources:
      requests:
        storage: 500Mi
  ```

  Apply `local-pvc.yaml` file:
  ```shell
  k apply -f local-pvc.yaml
  ```

  Check events:
  ```shell
  kubectl describe pvc local-pvc | grep -A3 Events
  ```

  Solution manifest file to create a pod called nginx is as follows into `nginx-pod.yaml` file:
  ```yaml
  ---
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
    labels:
      name: nginx
  spec:
    containers:
    - name: nginx
      image: nginx:alpine
      volumeMounts:
        - name: local-persistent-storage
          mountPath: /var/www/html
    volumes:
      - name: local-persistent-storage
        persistentVolumeClaim:
          claimName: local-pvc
  ```

  Apply it:
  ```shell
  k apply -f nginx-pod.yaml
  ```

</details>

**Useful official documentation**

- [Storage Classes in Kubernetes](https://kubernetes.io/docs/concepts/storage/storage-classes/)
- [Ephemeral Volumes in Kubernetes](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/)
- [Persistent Volumes in Kubernetes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

**Useful non-official documentation**

- None

## Application Deployment - 20%

ReplicaSets - A ReplicaSet’s purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.
Deployments - A Deployment provides declarative updates for Pods ReplicaSets. You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

ReplicaSets VS Replication Controller

Replica Set and Replication Controller do almost the same thing. Both of them ensure that a specified number of pod replicas are running at any given time. The difference comes with the usage of selectors to replicate pods. Replica Set use Set-Based selectors while replication controllers use Equity-Based selectors. ReplicaSets is the Replication Controller replacement.

### Use Kubernetes primitives to implement common deployment strategies (e.g. blue/green or canary)

Examples:
- <details><summary>Example_1: Using Blue/Green deployment (example_1):</summary>

  I would like to create `NS`:
  ```shell
  k create ns blue-green
  ```

  Create `blue` version of deployment with the command:
  ```shell
  k create -n blue-green deployment blue-app --image=nginx:1.19 --port=80 --replicas=4 --dry-run=client -oyaml > blue-app-deploy.yaml
  ```

  Set label with CLI, but you can add it manually:
  ```shell
  k label deployments.apps -n blue-green blue-app env=blue
  ```

  Get the deployment:
  ```shell
  kubectl get deployments -n blue-green --show-labels
  NAME       READY   UP-TO-DATE   AVAILABLE   AGE    LABELS
  blue-app   4/4     4            4           2m5s   app=blue-app,env=blue
  ```
  
  An example of `blue` version of deployment looks like (`k get deployments.apps -n blue-green blue-app -o yaml` command and deleted some no needed lines):
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: blue-app
      env: blue
    name: blue-app
    namespace: blue-green
  spec:
    replicas: 4
    selector:
      matchLabels:
        app: blue-app
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: blue-app
      spec:
        containers:
        - image: nginx:1.19
          name: nginx
          ports:
          - containerPort: 80
          resources: {}
  status: {}
  ```

  NOTE: To apply file you can use `k apply -f blue-app-deploy.yaml`.

  Create Kubernetes `service` with command:
  ```shell
  k expose -n blue-green deployment blue-app --name=blue-green-svc --type=NodePort --port=80 --target-port=80 --selector="app=blue-app"
  ```

  NOTE: Can be possible to use `ClusterIP`.

  Or, manually something like:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    labels:
      app: blue-app
      env: blue
    name: blue-green-svc
    namespace: blue-green
  spec:
    ports:
    - nodePort: 32298
      port: 80
      protocol: TCP
      targetPort: 80
    selector:
      env: blue
    type: NodePort
  status: {}
  ```

  NOTE: If you want to patch service, you can use the next command:
  ```shell
  k patch svc -n blue-green blue-green-svc -p '{"spec":{"type":"NodePort"}}'
  ```

  Getting svc:
  ```shell
  k get svc -n blue-green --show-labels
  NAME             TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE     LABELS
  blue-green-svc   NodePort   10.97.239.43   <none>        80:32298/TCP   2m14s   app=blue-app,env=blue
  ```

  NOTE: Now we can make `port-forward`, to expose svc on local env, for example:
  ```shell
  k port-forward -n blue-green services/blue-green-svc --address=0.0.0.0 8888:80
  ```

  Then, create a new `green` version of deployment with the command:
  ```shell
  k create -n blue-green deployment green-app --image=httpd:2.4 --port=80 --replicas=4 --dry-run=client -oyaml > green-app-deploy.yaml
  ```

  Set a label with CLI, but you can add it manually:
  ```shell
  k label deployments.apps -n blue-green green-app env=green
  ```

  An example of `green` version of deployment looks like:
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: green-app
      env: green
    name: green-app
    namespace: blue-green
  spec:
    replicas: 4
    selector:
      matchLabels:
        app: green-app
    strategy: {}
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: green-app
      spec:
        containers:
        - image: httpd:2.4
          name: httpd
          ports:
          - containerPort: 80
          resources: {}
  status: {}
  ```

  Check if `httpd` has started well:
  ```shell
  k port-forward -n blue-green deployments/green-app --address=0.0.0.0 9999:80
  ```

  Then, switch traffic from `blue` to `green`:
  ```shell
  kubectl patch service blue-green-svc -n blue-green -p '{"spec":{"selector":{"app": "green-app"}}}'
  
  kubectl patch service blue-green-svc -n blue-green -p '{"metadata":{"labels":{"app": "green-app"}}}'
  kubectl patch service blue-green-svc -n blue-green -p '{"metadata":{"labels":{"env": "green"}}}'
  ```

  Now, checking:
  ```shell
  k describe svc -n blue-green blue-green-svc
  ```

  NOTE: Now we can make `port-forward`, to expose svc on local env, for example:
  ```shell
  k port-forward -n blue-green services/blue-green-svc --address=0.0.0.0 9999:80
  ```

  Also, can get output from `curl` command.

</details>

- <details><summary>Example_2: Using Blue/Green deployment (example_2):</summary>

  The full stack (blue-deployment + geen-deployment + service) in one `b-g-deployment.yaml` file:
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: blue-deployment
    labels:
      app: my-app
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: my-app
        version: v1
    template:
      metadata:
        labels:
          app: my-app
          version: v1
      spec:
        containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
          volumeMounts:
          - name: workdir
            mountPath: /usr/share/nginx/html
        initContainers:
        - name: install
          image: busybox:1.28
          command:
          - /bin/sh
          - -c
          - "echo version-1 > /work-dir/index.html"
          volumeMounts:
          - name: workdir
            mountPath: "/work-dir"
        volumes:
        - name: workdir
          emptyDir: {}
  
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: blue-green-svc
    labels:
      app: my-app
  spec:
    type: ClusterIP
    ports:
    - name: http
      port: 80
      targetPort: 80
    selector:
      app: my-app
      version: v1
  
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: green-deployment
    labels:
      app: my-app
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: my-app
        version: v2
    template:
      metadata:
        labels:
          app: my-app
          version: v2
      spec:
        containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
          volumeMounts:
          - name: workdir
            mountPath: /usr/share/nginx/html
        initContainers:
        - name: install
          image: busybox:1.28
          command:
          - /bin/sh
          - -c
          - "echo version-2 > /work-dir/index.html"
          volumeMounts:
          - name: workdir
            mountPath: "/work-dir"
        volumes:
        - name: workdir
          emptyDir: {}
  ```
  Apply the stack:
  ```shell
  k apply -f b-g-deployment.yaml
  ```

  Make port-forvarding:
  ```shell
  k port-forward services/my-app-svc --address=0.0.0.0 8888:80
  ```

  Now, you can use `curl` and test your deployment:
  ```shell
  curl http://localhost:8888
  version-1
  ```

  Or, test if the deployment was successful from within a Pod:
  ```shell
  kubectl run -it --rm --restart=Never busybox --image=gcr.io/google-containers/busybox --command -- wget -qO- blue-green-svc
  ```

  Then, need to edit the `svc` to start using `green version of deployment`. You can use manual way or:
  ```shell
  kubectl patch service my-app-svc -p '{"spec":{"selector":{"version": "v2"}}}'
  ```

  Make port-forvarding:
  ```shell
  k port-forward services/my-app-svc --address=0.0.0.0 8888:80
  ```

  Now, you can use `curl` and test your deployment:
  ```shell
  curl http://localhost:8888
  version-2
  ```

  It's working good!

</details>

- <details><summary>Example_3: Using Blue/Green deployment (example_3):</summary>

  Let's look at the option when one deployment has already been created and it's necessary to make a new deployment to use the "Blue-Green" approach. This time, let's get deploy:
  ```shell
  k get deploy
  NAME           READY   UP-TO-DATE   AVAILABLE   AGE
  wonderful-v1   4/4     4            4           28s
  ```

  Then, save this `wonderful-v1` deploy to file, for example:
  ```shell
   k get deploy wonderful-v1 -o yaml > wonderful-v1.yaml
  ```

  Open the file and delete no needed lines and the file should looks like:
  ```shell
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: wonderful
    name: wonderful-v1
    namespace: default
  spec:
    replicas: 4
    selector:
      matchLabels:
        app: wonderful
        version: v1
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        labels:
          app: wonderful
          version: v1
      spec:
        containers:
        - image: httpd:alpine
          imagePullPolicy: IfNotPresent
          name: httpd
          resources: {}
        restartPolicy: Always
        securityContext: {}
  ```

  Create a new Deployment `wonderful-v2` which uses image `nginx:alpine` with `4` replicas. It's Pods should have labels - `app: wonderful` and `version: v2`.
  NOTE: The `wonderful-v1` app has a deployment with image `httpd:alpine`, but should be switched over to `nginx:alpine`. Once all new Pods are running, change the selector label of Service `wonderful` to `version: v2`. Finally scale down Deployment `wonderful-v1` to `0` replicas.

  So, make a copy of the file to have a backup:
  ```shell
  cp wonderful-v1.yaml wonderful-v2.yaml
  ```

  Edit `wonderful-v2.yaml` file and adopt it for `v2` version of the deployment:
  ```shell
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: wonderful
    name: wonderful-v2
    namespace: default
  spec:
    replicas: 4
    selector:
      matchLabels:
        app: wonderful
        version: v2
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        labels:
          app: wonderful
          version: v2
      spec:
        containers:
        - image: nginx:alpine
          imagePullPolicy: IfNotPresent
          name: httpd
          resources: {}
        restartPolicy: Always
        securityContext: {}
  ```

  Apply a new `v2` deployment:
  ```shell
  k apply -f wonderful-v2.yaml
  ```

  Get deployemnts:
  ```shell
  k get deployments.apps 
  NAME           READY   UP-TO-DATE   AVAILABLE   AGE
  wonderful-v1   4/4     4            4           15m
  wonderful-v2   4/4     4            4           11s
  ``` 

  Now, switch traffic to `v2` and adopt it to use a new app:
  ```shell
  k edit svc wonderful
  ```

  The service should be like the following:
  ```shell
  apiVersion: v1
  kind: Service
  metadata:
    labels:
      app: wonderful
    name: wonderful
    namespace: default
  spec:
    ports:
    - nodePort: 30080
      port: 80
      protocol: TCP
      targetPort: 80
    selector:
      app: wonderful
      version: v2
    sessionAffinity: None
    type: NodePort
  status:
    loadBalancer: {}
  ```

  And, scale down old version:
  ```
  k scale deploy wonderful-v1 --replicas=0
  ```

</details>

- <details><summary>Example_4: Using Canary deployment (example_1):</summary>

  I would like to create `NS`:
  ```shell
  k create ns canary
  ```

  Create `primary` version of deployment with the command:
  ```shell
  k create -n canary deployment primary-app --image=nginx:1.19 --port=80 --replicas=4 --dry-run=client -oyaml > primary-app-deploy.yaml
  ```

  Set a label with CLI, but you can add it manually:
  ```shell
  k label deployments.apps -n canary primary-app env=primary
  k label deployments.apps -n canary primary-app deployment=canary
  ```

  The example of `primary-app` deployment looks like:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: primary-app
      deployment: canary
      env: primary
    name: primary-app
    namespace: canary
  spec:
    replicas: 4
    selector:
      matchLabels:
        app: primary-app
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        labels:
          app: primary-app
      spec:
        containers:
        - image: nginx:1.19
          name: nginx
          ports:
          - containerPort: 80
            protocol: TCP
          resources: {}
        securityContext: {}
  status: {}
  ```

  Get the deployment:
  ```shell
  kubectl get deployments -n canary --show-labels
  NAME          READY   UP-TO-DATE   AVAILABLE   AGE     LABELS
  primary-app   4/4     4            4           5m19s   app=primary-app,deployment=canary,env=primary
  ```

  Or, you can add them manually.

  Then, let's expose deployment with `service` something like:
  ```shell
  k expose -n canary deployment primary-app --name=canary-svc --type=NodePort --port=80 --target-port=80 --selector="app=primary-app"
  ```

  NOTE: Can be possible to use `ClusterIP`.

  Or, manually something like:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    labels:
      app: primary-app
      deployment: canary
      env: primary
    name: canary-svc
    namespace: canary
  spec:
    ports:
    - nodePort: 30378
      port: 80
      protocol: TCP
      targetPort: 80
    selector:
      app: primary-app
    type: NodePort
  status: {}
  ```

  Also, you can create service by kubectl command:
  ```shell
  k create service nodeport canary-svc --tcp=80:80 --node-port=30378 --dry-run=client -o yaml
  ```

  Getting svc:
  ```shell
  k get svc -n canary --show-labels
  NAME         TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE   LABELS
  canary-svc   NodePort   10.97.177.59   <none>        80:30378/TCP   53s   app=primary-app,deployment=canary,env=primary
  ```

  NOTE: Now we can make `port-forward`, to expose svc on local env, for example:
  ```shell
  k port-forward -n canary services/canary-svc --address=0.0.0.0 8888:80
  ```

  Then, create `canary` version of deployment with the next command:
  ```shell
  k create -n canary deployment canary-app --image=httpd:2.4 --port=80 --replicas=2 --dry-run=client -oyaml > canary-app-deploy.yaml
  ```

  Set a label with CLI, but you can add it manually:
  ```shell
  k label deployments.apps -n canary canary-app env=canary
  k label deployments.apps -n canary canary-app deployment=canary
  ```

  Now, need to edit created `canary` deployment and need to set proper labels from `primary` deployment:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: primary-app
    name: canary-app
    namespace: canary
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: primary-app
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: primary-app
      spec:
        containers:
        - image: httpd:2.4
          name: httpd
          ports:
          - containerPort: 80
          resources: {}
  status: {}
  ```

  Replace deployment:
  ```shell
  k replace -f canary-app-deploy.yaml --force
  ```

  Get pods with needed labels:
  ```shell
  k get po -n canary --show-labels
  NAME                           READY   STATUS    RESTARTS   AGE     LABELS
  canary-app-8447898f59-5nkxt    1/1     Running   0          7m43s   app=primary-app,pod-template-hash=8447898f59
  canary-app-8447898f59-bjc2g    1/1     Running   0          7m43s   app=primary-app,pod-template-hash=8447898f59
  primary-app-567d564c6b-5b6zw   1/1     Running   0          115s    app=primary-app,pod-template-hash=567d564c6b
  primary-app-567d564c6b-n8d94   1/1     Running   0          51s     app=primary-app,pod-template-hash=567d564c6b
  primary-app-567d564c6b-phc9j   1/1     Running   0          51s     app=primary-app,pod-template-hash=567d564c6b
  primary-app-567d564c6b-zl4hc   1/1     Running   0          51s     app=primary-app,pod-template-hash=567d564c6b
  ```

  Or:
  ```shell
  k get po -n canary -l app=primary-app
  NAME                           READY   STATUS    RESTARTS   AGE
  canary-app-8447898f59-5nkxt    1/1     Running   0          9m29s
  canary-app-8447898f59-bjc2g    1/1     Running   0          9m29s
  primary-app-567d564c6b-5b6zw   1/1     Running   0          3m41s
  primary-app-567d564c6b-n8d94   1/1     Running   0          2m37s
  primary-app-567d564c6b-phc9j   1/1     Running   0          2m37s
  primary-app-567d564c6b-zl4hc   1/1     Running   0          2m37s
  ```

  One more check - svc and deployments:
  ```shell
  k get deploy,svc -n canary -l app=primary-app
  NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
  deployment.apps/canary-app    2/2     2            2           12m
  deployment.apps/primary-app   4/4     4            4           4h27m
  
  NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
  service/canary-svc   NodePort   10.97.177.59   <none>        80:30378/TCP   23m
  ```

  Make `port-forward` on service:
  ```shell
  k port-forward -n canary services/canary-svc --address=0.0.0.0 8888:80
  ```

  Now, checking stack, for this case you can use `curl` command in loop:
  ```shell
  while true; do sleep 1; curl http://localhost:8888; done
  ```

</details>

- <details><summary>Example_5: Using Canary deployment (example_2):</summary>

  The full stack (primary-deployment + canary-deployment + service) in one `canary-deployment.yaml` file:
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: primary-deployment
    labels:
      app: my-app
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: my-app
        version: v1
    template:
      metadata:
        labels:
          app: my-app
          version: v1
      spec:
        containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
          volumeMounts:
          - name: workdir
            mountPath: /usr/share/nginx/html
        initContainers:
        - name: install
          image: busybox:1.28
          command:
          - /bin/sh
          - -c
          - "echo version-1 > /work-dir/index.html"
          volumeMounts:
          - name: workdir
            mountPath: "/work-dir"
        volumes:
        - name: workdir
          emptyDir: {}
  
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: canary-svc
    labels:
      app: my-app
  spec:
    type: ClusterIP
    ports:
    - name: http
      port: 80
      targetPort: 80
    selector:
      app: my-app
  
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: canary-deployment
    labels:
      app: my-app
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: my-app
        version: v2
    template:
      metadata:
        labels:
          app: my-app
          version: v2
      spec:
        containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
          volumeMounts:
          - name: workdir
            mountPath: /usr/share/nginx/html
        initContainers:
        - name: install
          image: busybox:1.28
          command:
          - /bin/sh
          - -c
          - "echo version-2 > /work-dir/index.html"
          volumeMounts:
          - name: workdir
            mountPath: "/work-dir"
        volumes:
        - name: workdir
          emptyDir: {}
  ```

  Apply stacks:
  ```shell
  k apply -f canary-deployment.yaml
  ```

  Make port-forvarding:
  ```shell
  k port-forward services/canary-svc --address=0.0.0.0 8888:80
  ```

  Now, you can use `curl` and test your deployment:
  ```shell
  curl http://localhost:8888
  version-1
  version-1
  version-1
  version-1
  version-1
  version-1
  version-1
  version-2
  version-1
  version-1
  version-1
  version-1
  version-1
  version-1
  ```

  Or, test if the deployment was successful from within a Pod:
  ```shell
  kubectl run -it --rm --restart=Never busybox --image=gcr.io/google-containers/busybox --command -- wget -qO- canary-svc
  version-1
  version-1
  version-1
  version-1
  version-1
  version-1
  version-1
  version-2
  version-1
  version-1
  version-1
  version-1
  version-1
  version-1
  ```

  It's working good!

</details>

- <details><summary>Example_6: Using Canary deployment (example_3):</summary>

  Let's look at the option when one deployment has already been created and it's necessary to make a new deployment to use the "Canary" approach. This time, let's get deploy:
  ```shell
  k get deploy
  NAME           READY   UP-TO-DATE   AVAILABLE   AGE
  wonderful-v1   10/10   10           10          3m49s
  ```

  Then, save this `wonderful-v1` deploy to file, for example:
  ```shell
   k get deploy wonderful-v1 -o yaml > wonderful-v1.yaml
  ```

  Open the file and delete no needed lines and the file should looks like:
  ```shell
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: wonderful
    name: wonderful-v1
    namespace: default
  spec:
    replicas: 10
    selector:
      matchLabels:
        app: wonderful
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: wonderful
      spec:
        containers:
        - image: httpd:alpine
          imagePullPolicy: IfNotPresent
          name: httpd
          resources: {}
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        securityContext: {}
  ```

  The app has a Deployment with image `httpd:alpine`, but should be switched over to `nginx:alpine`.

  The switch should not happen fast or automatically, but using the Canary approach:
  - 20% of requests should hit the new image
  - 80% of requests should hit the old image

  For this create a new Deployment `wonderful-v2` which uses image `nginx:alpine`. The total amount of Pods of both Deployments combined should be 10.

  Copy the deployment:
  ```shell
  cp wonderful-v1.yaml wonderful-v2.yaml
  ```

  Then, open a new deployment (`wonderful-v2.yaml` file) and make changes. Something like:
  ```shell
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: wonderful
    name: wonderful-v2
    namespace: default
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: wonderful
    strategy:
      rollingUpdate:
        maxSurge: 25%
        maxUnavailable: 25%
      type: RollingUpdate
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: wonderful
      spec:
        containers:
        - image: nginx:alpine
          imagePullPolicy: IfNotPresent
          name: httpd
          resources: {}
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        securityContext: {}
  ```

  Apply the deployment:
  ```shell
  k apply -f wonderful-v2.yaml
  ```

  Edit `wonderful-v1` deployment and scale-down pods to `8`. Manual or through:
  ```shell
  k scale deployment wonderful-v1 --replicas=8
  ```

</details>

- <details><summary>Example_7: Using Recreate deployment (example_1):</summary>

  For exapmle, open `web-server-recreate-deployment.yaml` file and put the next data:
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: web-server
  spec:
    replicas: 4
    strategy:
      type: Recreate
    selector:
      matchLabels:
        app: httpd
    template:
      metadata:
        labels:
          app: httpd
      spec:
        containers:
        - name: httpd
          image: httpd:2.4.23-alpine
          ports:
          - containerPort: 80
            protocol: TCP
  ```

  Then, apply it:
  ```shell
  k apply -f web-server-recreate-deployment.yaml
  ```

  Get pods:
  ```shell
  k get po -l app=httpd
  ```

</details>

- <details><summary>Example_8: Using Rollingupdate deployment (example_1):</summary>

  For exapmle, open `web-server-rollingupdate-deployment.yaml` file and put the next data:
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: httpd
  spec:
    replicas: 4
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxUnavailable: 40%
        maxSurge: 10%
    minReadySeconds: 60
    selector:
      matchLabels:
        app: web-server
    template:
      metadata:
        labels:
          app: web-server
      spec:
        containers:
        - name: httpd
          image: httpd:2.4.23-alpine
          ports:
          - containerPort: 80
            protocol: TCP
          readinessProbe:
            httpGet:
              path: /
              port: 80
  ```
  
  Then, apply it:
  ```shell
  k apply -f web-server-rollingupdate-deployment.yaml
  ```

  Get pods:
  ```shell
  k get po -l app=web-server
  ```

</details>

**Useful official documentation**

- [Kubernetes deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

**Useful non-official documentation**

- [What is Kubernetes Blue Green Deployment?](https://www.geeksforgeeks.org/what-is-kubernetes-blue-green-deployment/)
- [What Are Blue-Green Deployments in Kubernetes?](https://spacelift.io/blog/blue-green-deployment-kubernetes)
- [Blue-Green Deployment Using Kubernetes](https://blog.stackademic.com/blue-green-deployment-using-kubernetes-4fbd023a19c5)
- [Kubernetes Blue/Green Deployment Tutorial](https://github.com/ianlewis/kubernetes-bluegreen-deployment-tutorial)
- [Blue-Green Deployment in Kubernetes](https://medium.com/@muppedaanvesh/blue-green-deployment-in-kubernetes-76f9153e0805)
- [Blue-Green Deployments with Kubernetes: A Comprehensive Guide](https://medium.com/cloud-native-daily/blue-green-deployments-with-kubernetes-a-comprehensive-guide-5d196dad1976)
- [Implementing Canary Deployment in Kubernetes](https://medium.com/@muppedaanvesh/implementing-canary-deployment-in-kubernetes-0be4bc1e1aca)
- [Complete Guide On Kubernetes Canary Deployment](https://zeet.co/blog/kubernetes-canary-deployment)
- [Create a Kubernetes Canary deployment](https://developer.harness.io/docs/continuous-delivery/deploy-srv-diff-platforms/kubernetes/kubernetes-executions/create-a-kubernetes-canary-deployment/)
- [Using Canary Deployments in Kubernetes](https://earthly.dev/blog/canary-deployment-in-k8s/)
- [Implementing Canary Deployments in Kubernetes: A Comprehensive Guide](https://medium.com/@lexitrainerph/implementing-canary-deployments-in-kubernetes-a-comprehensive-guide-b0ab988e5331)
- [Canary Deployments in Kubernetes: An In-Depth Guide](https://overcast.blog/canary-deployments-in-kubernetes-an-in-depth-guide-81ede6a28977)

### Understand Deployments and how to perform rolling updates

Examples:
- <details><summary>Example_1: Using rolling updates (deprecated way):</summary>

  1. Make changes in YAML file and run:
    ```shell
    k apply -f deploy-definition.yaml
    ```
  2. Update only needed param, for example - `image` only:
    ```shell
    k set image deployment/my-deploy-1 nginx-container=nginx:1.19 --record
    ```

    Where:
    - `my-deploy-1` - The deployment name.
    - `nginx-container` - The container name.
    - `nginx:1.19` - The needed image name with tag.

  Let's create a new deployment:
  ```shell
  kubectl create deployment my-deploy-1 --image=nginx:1.17
  ```

  Let's update image:
  ```shell
  kubectl set image deployment my-deploy-1 nginx=nginx:1.18 --record
  ```

  NOTE: Flag `--record` has been deprecated, `--record` will be removed in the future

  Getting events from deployment:
  ```shell
  k describe deploy my-deploy-1
  ```

</details>

- <details><summary>Example_2: Using rolling updates (new one):</summary>

  Create the deployment:
  ```shell
  k create deployment nginx --image=nginx:1.16.0 --replicas=1
  ```

  Check the history of the `nginx` deployment:
  ```shell
  k rollout history deployment nginx
  
  deployment.apps/nginx
  REVISION  CHANGE-CAUSE
  1         <none>
  
  ```

  Update the image on deployment:
  ```shell
  k set image deployment/nginx nginx=nginx:latest
  ```

  Annotate the deployment now and create the history:
  ```shell
  k annotate deployment nginx kubernetes.io/change-cause="version change to 1.16.0 to latest" --overwrite=true
  ```

  Check the history
  ```shell
  k rollout history deployment nginx
  
  deployment.apps/nginx
  REVISION  CHANGE-CAUSE
  1         <none>
  2         version change to 1.16.0 to latest
  
  ```

  Revert changes back, for example:
  ```shell
  k rollout undo deployment nginx --to-revision=1
  ```

</details>

- <details><summary>Example_3: Using rollbacks:</summary>

  Rollout command:
  ```shell
  k rollout status deployment/my-deploy-1
  ```

  To get history of rollouts:
  ```shell
  k rollout history deployment/my-deploy-1
  ```

  Make rollout:
  ```shell
  k rollout undo deployment/my-deploy-1
  ```

  Or:
  ```shell
  kubectl rollout undo deployment my-deploy-1 --to-revision=1
  ```

</details>

- <details><summary>Example_4: Using maxSurge + maxUnavailable in Deployments:</summary>

  - `maxSurge` is an optional field that specifies the maximum number of Pods that can be created over the desired number of Pods. The value can be an absolute number (for example, 5) or a percentage of desired Pods (for example, 10%). The value cannot be 0 if MaxUnavailable is 0. The absolute number is calculated from the percentage by rounding up. The default value is 25%.
  - `maxUnavailable` is an optional field that specifies the maximum number of Pods that can be unavailable during the update process. The value can be an absolute number (for example, 5) or a percentage of desired Pods (for example, 10%). The absolute number is calculated from percentage by rounding down. The value cannot be 0 if .spec.strategy.rollingUpdate.maxSurge is 0. The default value is 25%.

  Let's create a new deployment:
  ```shell
  k create deployment my-deploy --image=httpd:2.4 --port=80 --replicas=7 --dry-run=client -oyaml > my-deploy.yaml
  ```

  Open `my-deploy.yaml` file and put `maxSurge` & `maxUnavailable` parameters:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: my-deploy
    name: my-deploy
  spec:
    replicas: 7
    selector:
      matchLabels:
        app: my-deploy
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxUnavailable: 2
        maxSurge: 2
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: my-deploy
      spec:
        containers:
        - image: httpd:2.4
          name: httpd
          ports:
          - containerPort: 80
          resources: {}
  status: {}
  ```

  Apply the file:
  ```shell
  k apply -f ./my-deploy.yaml
  ```

</details>

**Useful official documentation**

- [Rolling update Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)
- [Rolling back a Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)

**Useful non-official documentation**

- [Rolling Update & Recreate Deployment Strategies in Kubernetes](https://medium.com/@muppedaanvesh/rolling-update-recreate-deployment-strategies-in-kubernetes-%EF%B8%8F-327b59f27202)

### Use the Helm package manager to deploy existing packages

Examples:
- <details><summary>Example_1: Install & use helm:</summary>

  Run the next commands:
  ```shell
  curl https://baltocdn.com/helm/signing.asc | apt-key add -
  apt-get install apt-transport-https --yes
  echo "deb https://baltocdn.com/helm/stable/debian/ all main" | tee /etc/apt/sources.list.d/helm-stable-debian.list
  apt-get update && apt-get install helm
  ```

  Now, you can add some repo:
  ```shell
  helm repo add bitnami https://charts.bitnami.com/bitnami
  ```

  Search packege, for example:
  ```shell
  helm search repo joomla
  ```

  To get list of repos:
  ```shell
  helm repo list
  ```

  Now, let's install something, for example:
  ```shell
  helm install drupal bitnami/drupal
  ```

  To get releases:
  ```shell
  helm list
  ```

  To delete installed release:
  ```shell
  helm uninstall drupal
  ```

  NOTE: Use `-n NAMESPACE` if the release has deployed to a specific one.

  To download some release and store it on host you can use the next command:
  ```shell
  helm pull --untar bitnami/apache
  ```

  To deploy your release:
  ```shell
  helm install mywebapp ./apache
  ```

  To write the list of all Helm releases in the cluster into `/root/releases` file, for example:
  ```shell
  helm ls -A > /root/releases
  ```

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- [Installing Helm](https://helm.sh/docs/intro/install/)

### Kustomize

This part is out of the certification, but let's discuss the point a bit:
- `kustomize` lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is.
- `kustomize` targets kubernetes; it understands and can patch kubernetes style API objects. It's like make, in that what it does is declared in a file, and it's like sed, in that it emits edited text.

So, kustomize is a command-line configuration manager for Kubernetes objects. Integrated with kubectl since 1.14, it allows you to make declarative changes to your configurations without touching a template. For example, you can combine pieces from different sources, keep your customizations -- or kustomizations, as the case may be -- in source control, and create overlays for specific situations. Kustomize enables you to do that by creating a file that ties everything together, or optionally includes "overrides" for individual parameters.

Benefits of Using Kustomize:
1. Reusability: Kustomize allows you to reuse one base file across all of your environments (development, staging, production) and then overlay unique specifications for each.
2. Fast Generation: Since Kustomize has no templating language, you can use standard YAML to quickly declare your configurations.
3. Easier to Debug: YAML itself is easy to understand and debug when things go wrong. Pair that with the fact that your configurations are isolated in patches, and you’ll be able to triangulate the root cause of performance issues in no time. Simply compare performance to your base configuration and any other variations that are running.

Examples:
- <details><summary>Example_1: Customization of kubernetes YAML configurations with kubectl kustomize:</summary>

  To get help, use:
  ```shell
  kubectl kustomize -h
  ```

</details>

**Useful official documentation**

- [Kubernetes Kustomize] (https://kubernetes.io/docs/reference/kubectl/generated/kubectl_kustomize/)

**Useful non-official documentation**

- [Kustomize Tutorial: Creating a Kubernetes app out of multiple pieces](https://www.mirantis.com/blog/introduction-to-kustomize-part-1-creating-a-kubernetes-app-out-of-multiple-pieces#what)
- [Kustomize: Simplifying Kubernetes Configuration Management](https://medium.com/@wadexu007/kustomize-simplifying-kubernetes-configuration-management-9b5d9c1169c3)
- [Kustomize Tutorial: Comprehensive Guide For Beginners](https://devopscube.com/kustomize-tutorial/)
- [Kubernetes Kustomize Tutorial](https://antonputra.com/kubernetes/kubernetes-kustomize-tutorial/)
- [Kustomize examples](https://github.com/viadee/kustomize-examples)


## Application Observability and Maintenance - 15%

### Understand API deprecations

Examples:
- <details><summary>Example_1: Find and fix deprecations API in PODs/Deployments:</summary>

  Get API version of deploy:
  ```shell
  k explain deploy
  ```

  Get API version of job:
  ```shell
  kubectl explain job | grep -Ei group
  ```

  Get API version of POD:
  ```shell
  k explain pod
  ```

  Or, better one:
  ```shell
  kubectl api-resources
  ```

  Show resources per namespace or non-namespaced resources:
  ```shell
  k api-resources --namespaced=false
  ```

  Get `API` resources which support `list` and `get` type of verbs:
  ```shell
  k api-resources --verbs=list,get
  ```

  Print the supported API resources with a specific APIGroup:
  ```shell
  k api-resources --api-group=extensions
  k api-resources --api-group=rbac.authorization.k8s.io
  ```

</details>

- <details><summary>Example_2: Enabling/Disabling API Groups in Kubernetes:</summary>

  Enable the `v1alpha1` version for `rbac.authorization.k8s.io` API group on the `controlplane` node. Opening `/etc/kubernetes/manifests/kube-apiserver.yaml` file and put `--runtime-config` something like this:
  ```yaml
   .....
   - command:
    - kube-apiserver
    - --advertise-address=10.18.17.8
    - --allow-privileged=true
    - --authorization-mode=Node,RBAC
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --enable-admission-plugins=NodeRestriction
    - --enable-bootstrap-token-auth=true
    - --runtime-config=rbac.authorization.k8s.io/v1alpha1
    .....
  ```
  
  Adding str inside `/etc/kubernetes/manifests/kube-apiserver.yaml`:
  ```yaml
  ....
  --runtime-config=batch/v2alpha1
  ....
  ```
  
  Now, let's install the `kubectl convert` plugin on the `controlplane` node:
  ```shell
  curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert
  chmod +x kubectl-convert
  mv kubectl-convert /usr/local/bin/kubectl-convert
  ```
  
  Now, you will be able to convert any resources, for example:
  ```shell
  kubectl-convert -f ingress-old.yaml --output-version networking.k8s.io/v1
  ```

</details>

**Useful official documentation**

- [The Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)

**Useful non-official documentation**

- None

### Implement probes and health checks

A common pattern for liveness probes is to use the same low-cost HTTP endpoint as for readiness probes, but with a higher failureThreshold. This ensures that the pod is observed as not-ready for some period of time before it is hard killed.

The kubelet uses readiness probes to know when a container is ready to start accepting traffic. A Pod is considered ready when all of its containers are ready. One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.

The kubelet uses startup probes to know when a container application has started. If such a probe is configured, liveness and readiness probes do not start until it succeeds, making sure those probes don't interfere with the application startup. This can be used to adopt liveness checks on slow starting containers, avoiding them getting killed by the kubelet before they are up and running.

Examples:
- <details><summary>Example_1: Create readiness probe with httpGet:</summary>

  Create a new pod to play around with readiness probe, for example:
  ```shell
  k run readiness-probe-httpget --image=nginx --dry-run=client -oyaml > readiness-probe-httpget.yaml
  ```

  Edit created file and add `httpGet` readiness probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: readiness-probe-httpget
    name: readiness-probe-httpget
  spec:
    containers:
    - image: nginx
      name: readiness-probe-httpget
      ports:
      - containerPort: 8080
        protocol: TCP
      readinessProbe:
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 2
        periodSeconds: 8
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f readiness-probe-httpget.yaml
  ```

  Checking:
  ```shell
  k get po readiness-probe-httpget
  ```

</details>

- <details><summary>Example_2: Create readiness probe with exec:</summary>

  Create a new pod to play around with readiness probe, for example:
  ```shell
  k run readiness-probe-exec --image=nginx --dry-run=client -oyaml > readiness-probe-exec.yaml
  ```

  Edit created file and add `exec` readiness probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: readiness-probe-exec
    name: readiness-probe-exec
  spec:
    containers:
    - image: nginx
      name: readiness-probe-exec
      ports:
      - containerPort: 8080
        protocol: TCP
      readinessProbe:
        exec:
          command:
          - "nc"
          - "-z"
          - "-v" 
          - "localhost"
          - "8080"
        initialDelaySeconds: 5
        periodSeconds: 5
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f readiness-probe-exec.yaml
  ```

  Checking:
  ```shell
  k get po readiness-probe-exec
  ```

</details>

- <details><summary>Example_3: Create readiness probe with tcpSocket:</summary>

  Create a new pod to play around with readiness probe, for example:
  ```shell
  k run readiness-probe-tcpsocket --image=nginx --dry-run=client -oyaml > readiness-probe-tcpsocket.yaml
  ```

  Edit created file and add `tcpSocket` readiness probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: readiness-probe-tcpsocket
    name: readiness-probe-tcpsocket
  spec:
    containers:
    - image: nginx
      name: readiness-probe-tcpsocket
      ports:
      - containerPort: 8080
        protocol: TCP
      readinessProbe:
        tcpSocket:
          port: 8080
        initialDelaySeconds: 15
        periodSeconds: 10
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f readiness-probe-tcpsocket.yaml
  ```

  Checking:
  ```shell
  k get po readiness-probe-tcpsocket
  ```

</details>

- <details><summary>Example_4: Create liveness probe with httpGet:</summary>

  Create a new pod to play around with liveness probe, for example:
  ```shell
  k run liveness-probe-httpget --image=nginx --dry-run=client -oyaml > liveness-probe-httpget.yaml
  ```

  Edit created file and add `httpGet` liveness probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: liveness-probe-httpget
    name: liveness-probe-httpget
  spec:
    containers:
    - image: nginx
      name: liveness-probe-httpget
      ports:
      - containerPort: 8080
        protocol: TCP
      livenessProbe:
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 30 
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f liveness-probe-httpget.yaml
  ```

  Checking:
  ```shell
  k get po liveness-probe-httpget
  ```

</details>

- <details><summary>Example_5: Create liveness probe with exec:</summary>

  Create a new pod to play around with liveness probe, for example:
  ```shell
  k run liveness-probe-exec --image=nginx --dry-run=client -oyaml > liveness-probe-exec.yaml
  ```

  Edit created file and add `exec` liveness probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: liveness-probe-exec
    name: liveness-probe-exec
  spec:
    containers:
    - image: nginx
      name: liveness-probe-exec
      ports:
      - containerPort: 8080
        protocol: TCP
      readinessProbe:
        exec:
          command:
            - "nc"
            - "-z"
            - "-v"
            - "localhost"
            - "8080"
        initialDelaySeconds: 5
        periodSeconds: 5
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f liveness-probe-exec.yaml
  ```

  Checking:
  ```shell
  k get po liveness-probe-exec
  ```

</details>

- <details><summary>Example_6: Create liveness probe with tcpSocket:</summary>

  Create a new pod to play around with liveness probe, for example:
  ```shell
  k run liveness-probe-tcpsocket --image=nginx --dry-run=client -oyaml > liveness-probe-tcpsocket.yaml
  ```

  Edit created file and add `tcpSocket` liveness probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: liveness-probe-tcpsocket
    name: liveness-probe-tcpsocket
  spec:
    containers:
    - image: nginx
      name: liveness-probe-tcpsocket
      ports:
      - containerPort: 8080
        protocol: TCP
      readinessProbe:
        tcpSocket:
          port: 8080
        initialDelaySeconds: 15
        periodSeconds: 10
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f liveness-probe-tcpsocket.yaml
  ```

  Checking:
  ```shell
  k get po liveness-probe-tcpsocket
  ```

</details>

- <details><summary>Example_7: Create startup probe with httpGet:</summary>

  Create a new pod to play around with startup probe, for example:
  ```shell
  k run startup-probe-httpget --image=nginx --dry-run=client -oyaml > startup-probe-httpget.yaml
  ```

  Edit created file and add `httpGet` startup probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: startup-probe-httpget
    name: startup-probe-httpget
  spec:
    containers:
    - image: nginx
      name: startup-probe-httpget
      ports:
      - containerPort: 8080
        protocol: TCP
      startupProbe:
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 3
        periodSeconds: 15
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f startup-probe-httpget.yaml
  ```

  Checking:
  ```shell
  k get po startup-probe-httpget
  ```

</details>

- <details><summary>Example_8: Create startup probe with exec:</summary>

  Create a new pod to play around with startup probe, for example:
  ```shell
  k run startup-probe-exec --image=nginx --dry-run=client -oyaml > startup-probe-exec.yaml
  ```

  Edit created file and add `exec` startup probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: startup-probe-exec
    name: startup-probe-exec
  spec:
    containers:
    - image: nginx
      name: startup-probe-exec
      ports:
      - containerPort: 8080
        protocol: TCP
      startupProbe:
        exec:
          command:
            - "nc"
            - "-z"
            - "-v"
            - "localhost"
            - "8080"
        initialDelaySeconds: 5
        periodSeconds: 5
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f startup-probe-exec.yaml
  ```

  Checking:
  ```shell
  k get po startup-probe-exec
  ```

</details>

- <details><summary>Example_9: Create startup probe with tcpSocket:</summary>

  Create a new pod to play around with startup probe, for example:
  ```shell
  k run startup-probe-tcpsocket --image=nginx --dry-run=client -oyaml > startup-probe-tcpsocket.yaml
  ```

  Edit created file and add `tcpSocket` startup probe on `8080` port: 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: startup-probe-tcpsocket
    name: startup-probe-tcpsocket
  spec:
    containers:
    - image: nginx
      name: startup-probe-tcpsocket
      ports:
      - containerPort: 8080
        protocol: TCP
      startupProbe:
        tcpSocket:
          port: 8080
        initialDelaySeconds: 15
        periodSeconds: 10
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply file:
  ```shell
  k apply -f startup-probe-tcpsocket.yaml
  ```

  Checking:
  ```shell
  k get po startup-probe-tcpsocket
  ```

</details>

**Useful official documentation**

- [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

**Useful non-official documentation**

- None

### Use built-in CLI tools to monitor Kubernetes applications

For certification, only needs - `metrics server`. Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines. Metrics Server collects resource metrics from Kubelets and exposes them in Kubernetes apiserver through Metrics API for use by Horizontal Pod Autoscaler and Vertical Pod Autoscaler.


Examples:
- <details><summary>Example_1: Install metrics server:</summary>

  To install `metrics-server`, use:
  ```shell
  kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
  ```

  If you have HA (High availability) cluster, use this:
  ```shell
  kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/high-availability.yaml
  ```

  Simple check:
  ```shell
  kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes
  ```

</details>

- <details><summary>Example_2: Get node metrics:</summary>

  If you want to get CPU & Mem usage on nodes:
  ```shell
  k top node
  ```

</details>

- <details><summary>Example_3: Get POD metrics:</summary>

  If you want to get CPU & Mem usage on PODs:
  ```shell
  k top pod
  ```

</details>

### Utilize container logs

Examples:
- <details><summary>Example_1: Getting logs:</summary>

  To get logs from POD, use:
  ```shell
  k logs <POD_ID_or_NAME>
  ```

  To get logs from deployment, use:
  ```shell
  k logs <DEPLOYMENT_NAME_or_ID>
  ```

  Get logs newer than a relative duration like `1h`:
  ```shell
  k logs --since=1m deployments/nginx
  ```

  Get log from specific container if you have multiple container inside the POD:
  ```
  k logs my-pod -c my-container-1 --tail=10
  ```

  If you want to get all logs from all containers inside the POD, you can use:
  ```
  k logs my-pod --all-containers=true
  ```

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Debugging in Kubernetes

Examples:
- <details><summary>Example_1: Using logs:</summary>

  To get logs from POD, use:
  ```shell
  k logs <POD_ID_or_NAME>
  ```

  To get logs from deployment, use:
  ```shell
  k logs <DEPLOYMENT_NAME_or_ID>
  ```

  Get logs newer than a relative duration like `1h`:
  ```shell
  k logs --since=1m deployments/nginx
  ```

  Get log from specific container if you have multiple container inside the POD:
  ```
  k logs my-pod -c my-container-1 --tail=10
  ```

  If you want to get all logs from all containers inside the POD, you can use:
  ```
  k logs my-pod --all-containers=true
  ```

</details>

- <details><summary>Example_2: Using describe command in kubectl:</summary>

  To describe POD, use:
  ```shell
  k describe po <POD_ID_or_NAME>
  ```

  To get logs from deployment, use:
  ```shell
  k describe deploy <DEPLOYMENT_NAME_or_ID>
  ```

</details>

- <details><summary>Example_3: Using crictl:</summary>

  Getting ID of container:
  ```shell
  crictl ps -a | grep api
  ```

  Check logs:
  ```shell
  crictl logs fbb80dac7429e
  ```

  Where:
  - `fbb80dac7429e` - ID of container.

</details>

- <details><summary>Example_4: Check logs on node(s):</summary>


</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None


## Application Environment, Configuration and Security - 25%

### Discover and use resources that extend Kubernetes (CRD, Operators)

A custom resource is an object that extends the Kubernetes API or allows you to introduce your own API into a project or a cluster. A custom resource definition (CRD) file defines your own object kinds and lets the API Server handle the entire lifecycle.

Examples:
- <details><summary>Example_1: Using Custom Resource Definition (CRD):</summary>

  CRDs offer several benefits and use cases within Kubernetes environments:
  - Domain-specific abstractions: CRDs allow users to define resources that align closely with their specific domain or application requirements. This enables developers to work at a higher level of abstraction, increasing productivity and reducing complexity.
  - Simplified API: CRDs simplify the management and configuration of custom resources by providing a declarative API. This allows users to interact with custom resources using familiar Kubernetes concepts, such as creating, updating, deleting, and querying resources through the Kubernetes API server.
  - Extensibility: Kubernetes provides a solid foundation, but not every use case can be fully addressed by the built-in resources. CRDs offer a flexible mechanism for extending Kubernetes with custom functionality, empowering users to build tailored solutions on top of the platform.

  This topic's out of that certificate! But let's take a look at the simple example.

  Create a CustomResourceDefinition manifest file for an Operator with the following specifications. Open `operator-crd.yml` and put the next:
  ```yaml
  apiVersion: apiextensions.k8s.io/v1
  kind: CustomResourceDefinition
  metadata:
    # name must match the spec fields below, and be in the form: <plural>.<group>
    name: operators.stable.example.com
  spec:
    group: stable.example.com
    versions:
      - name: v1
        served: true
        # One and only one version must be marked as the storage version.
        storage: true
        schema:
          openAPIV3Schema:
            type: object
            properties:
              spec:
                type: object
                properties:
                  email:
                    type: string
                  name:
                    type: string
                  age:
                    type: integer
    scope: Namespaced
    names:
      plural: operators
      singular: operator
      # kind is normally the CamelCased singular type. Your resource manifests use this.
      kind: Operator
      shortNames:
      - op
  ```

  Apply the configureation:
  ```shell
  k apply -f operator-crd.yml
  ```

  Create custom object from the CRD:
  ```yaml
  apiVersion: stable.example.com/v1
  kind: Operator
  metadata:
    name: operator-sample
  spec:
    email: operator-sample@stable.example.com
    name: "operator sample"
    age: 30
  ```

  Listing operator:
  ```shell
  kubectl get operators
  kubectl get operator
  kubectl get op
  ```

  To get a list of all installed CRDs you can with the next command:
  ```shell
  k get crd
  ```

  To delete crd, use:
  ```shell
  k delete crd operator-sample
  ```

</details>

**Useful official documentation**

- [Custom Resources in Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)

**Useful non-official documentation**

- [A Hands-On Guide to Kubernetes Custom Resource Definitions (CRDs) With a Practical Example](https://medium.com/@muppedaanvesh/a-hand-on-guide-to-kubernetes-custom-resource-definitions-crds-with-a-practical-example-%EF%B8%8F-84094861e90b)

### Understand authentication, authorization and admission controllers

To access and manage any Kubernetes resource or object in the cluster, we need to access a specific API endpoint on the API server. Each access request goes through the following three stages:

**1. Authentication** - Logs in a user.

Kubernetes does not have an object called user, nor does it store usernames or other related details in its object store. However, even without that, Kubernetes can use usernames for access control and request logging.

Kubernetes has two kinds of users:

- **Normal Users** They are managed outside of the Kubernetes cluster via independent services like User/Client Certificates, a file listing usernames/passwords, Google accounts, etc.
- **Service Accounts** With Service Account users, in-cluster processes communicate with the API server to perform different operations. Most of the Service Account users are created automatically via the API server, but they can also be created manually. The Service Account users are tied to a given Namespace and mount the respective credentials to communicate with the API server as Secrets.

For authentication, Kubernetes uses different authentication modules:

- **Client Certificates** To enable client certificate authentication, we need to reference a file containing one or more certificate authorities by passing the `--client-ca-file=SOMEFILE` option to the API server. The certificate authorities mentioned in the file would validate the client certificates presented to the API server. A demonstration video covering this topic is also available at the end of this chapter.
- **Static Token File** We can pass a file containing pre-defined bearer tokens with the `--token-auth-file=SOMEFILE` option to the API server. Currently, these tokens would last indefinitely, and they cannot be changed without restarting the API server.
- **Bootstrap Tokens** This feature is currently in beta status and is mostly used for bootstrapping a new Kubernetes cluster.
- **Static Password File** It is similar to Static Token File. We can pass a file containing basic authentication details with the `--basic-auth-file=SOMEFILE` option. These credentials would last indefinitely, and passwords cannot be changed without restarting the API server.
- **Service Account Tokens** This is an automatically enabled authenticator that uses signed bearer tokens to verify the requests. These tokens get attached to Pods using the ServiceAccount Admission Controller, which allows in-cluster processes to talk to the API server.
- **OpenID Connect Tokens** OpenID Connect helps us connect with OAuth2 providers, such as Azure Active Directory, Salesforce, Google, etc., to offload the authentication to external services.
- **Webhook Token Authentication** With Webhook-based authentication, verification of bearer tokens can be offloaded to a remote service.
- **Authenticating Proxy** If we want to program additional authentication logic, we can use an authenticating proxy.

**2. Authorization** - Authorizes the API requests added by the logged-in user.

After a successful authentication, users can send the API requests to perform different operations. Then, those API requests get authorized by Kubernetes using various authorization modules:

- Node Authorizer
- Attribute-Based Access Control (ABAC) Authorizer
- Webhook Authorizer
- Role-Based Access Control (RBAC) Authorizer

**Role** - With Role, we can grant access to resources within a specific Namespace.
**ClusterRole** - The ClusterRole can be used to grant the same permissions as Role does, but its scope is cluster-wide.

**RoleBinding** - It allows us to bind users to the same namespace as a Role. We could also refer a ClusterRole in RoleBinding, which would grant permissions to Namespace resources defined in the ClusterRole within the RoleBinding’s Namespace.
**ClusterRoleBinding**- It allows us to grant access to resources at a cluster-level and to all Namespaces.

**3. Admission Control** - Software modules that can modify or reject the requests based on some additional checks, like a pre-set Quota. Admission control is used to specify granular access control policies, which include allowing privileged containers, checking on resource quota, etc. To use admission controls, we must start the Kubernetes API server with the `--enable-admission-plugins`, which takes a comma-delimited, ordered list of controller names: `--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurityPolicy,DefaultStorageClass`.

Examples:
- <details><summary>Example_1: Using kubeconfig file to work with Kubernetes:</summary>

  To view Kubernetes config:
  ```shell
  k config view
  ```

  Or, if you have custome kubeconfig file, for example:
  ```shell
  k config get-clusters --kubeconfig=my-custom-kubeconfig
  ```

</details>

- <details><summary>Example_2: Using Authorication mode in Kubernetes API server:</summary>

  Checking which auth-mode kube-apiserver uses:
  ```shell
  cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep -Ei "authorization-mode"
    - --authorization-mode=Node,RBAC
  ```
  
  NOTE: Possible to use: `AlwaysAllow`, `NODE`, `ABAC`, `RBAC`, `Webhook`, `AlwaysDeny`.

</details>

- <details><summary>Example_3: Using RBAC to work with Kubernetes (roles and role bindings):</summary>

  To view roles in Kubernetes:
  ```shell
  k get roles
  ```

  Create role & rolebinding:
  ```shell
  k create role role_name --verb=get,list,watch --resource=pods
  k create rolebinding role_name_binding --role=role_name --user=captain --group=group1
  ```

  Verify:
  ```shell
  k auth can-i get pods --as captain -n kube-public
  k auth can-i list pods --as captain -n default
  ```

  One more example with role, open `read-only-role.yaml` file and put:
  ```yaml
  ---
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: read-only
  rules:
  - apiGroups:
    - ""
    resources:
    - pods
    - services
    verbs:
    - list
    - get
    - watch
  - apiGroups:
    - apps
    resources:
    - deployments
    verbs:
    - list
    - get
    - watch
  
  ---
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: read-only-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: read-only
  subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: captainua
  ```

  Apply the file:
  ```shell
  k apply -f read-only-role.yaml
  ```

</details>

- <details><summary>Example_4: Using RBAC to work with Kubernetes (cluster roles and cluster role bindings):</summary>

  To view cluster roles in Kubernetes:
  ```shell
  k get clusterrole
  ```

  Create clusterrole & clusterrolebinding:
  ```shell
  k create clusterrole cluster_role --verb=get,list,watch --resource=pods
  k create clusterrolebinding cluster_role_binding --clusterrole=cluster_role --user=cap
  ```
  
  Verify:
  ```shell
  k auth can-i list pods --as cap -n kube-public
  k auth can-i list pods --as cap -n default
  ```

</details>

- <details><summary>Example_5: Working with Service Account and RBAC:</summary>

  Get cluster roles:
  ```shell
  k get clusterrole
  ```

  Create Service Account and RBAC:
  ```shell
  k -n name_space_1 create sa ser_acc
  k create clusterrolebinding ser_acc-view --clusterrole view --serviceaccount name_space_1:ser_acc
  ```

  Where: 
  - `name_space_1` - NS name.
  - `ser_acc` - Service account name.

  Verify:
  ```shell
  k auth can-i update deployments --as system:serviceaccount:name_space_1:ser_acc -n default
  k auth can-i update deployments --as system:serviceaccount:name_space_1:ser_acc -n name_space_1
  ```

</details>

- <details><summary>Example_6: Using Admission controllers in Kubernetes:</summary>

  Checking which controllers are in use:
  ```shell
  grep enable-admission-plugins /etc/kubernetes/manifests/kube-apiserver.yaml
    - --enable-admission-plugins=NodeRestriction
  ```

  So, for example `NodeRestriction`.

  Let's add `NamespaceAutoProvision` controller and play around with it. Opening `/etc/kubernetes/manifests/kube-apiserver.yaml` and put the next data:
  ```yaml
  ...
  spec:
  containers:
  - command:
    - kube-apiserver
    - --advertise-address=192.0.180.9
    - --allow-privileged=true
    - --authorization-mode=Node,RBAC
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --enable-admission-plugins=NodeRestriction,NamespaceAutoProvision
  ...
  ```

  That controller allows you to auto-create namespaces. Let's check it! 
  ```shell
  kubectl run nginx --image nginx -n this-ns-doesnt-exist
  pod/nginx created
  ```

  Also, you can disable controllers, for example, - let's disable `DefaultStorageClass` in `/etc/kubernetes/manifests/kube-apiserver.yaml` and put the next data:
  ```yaml
  ...
  - --disable-admission-plugins=DefaultStorageClass
  ...
  ```
  
  To verify:
  ```shell
  ps -ef | grep kube-apiserver | grep admission-plugins
  ```
  
  *NOTE*: To get list plugins you can use:
  ```shell
  ps -ef | grep apiserver
  ```
  Getting plugins:
  ```shell
  /proc/15501/exe -h | grep -Ei plugins
  ```
  Where `15501` - PID ID of the process. 

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- [Kubernetes Access Control with Authentication, Authorization & Admission Control](https://blog.kubesimplify.com/kubernetes-access-control-with-authentication-authorization-admission-control)

### Understand requests, limits, quotas

If the node where a Pod is running has enough of a resource available, it's possible (and allowed) for a container to use more resource than its `request` for that resource specifies. However, a container is not allowed to use more than its resource `limit`.

A resource quota, defined by a `ResourceQuota` object, provides constraints that limit aggregate resource consumption per namespace. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that namespace.

`ResourceQuota` applies restriction with respect to CPU, memory for workloads and no of objects that can be created in a namespace. 
`LimitRange` defines default, maximum, minimum of CPU and memory consumption by workloads in a namespace. If you have a quota applied in a namespace, every Pod must request for resources like CPU and memory in their manifests. Otherwise the Pod creation will fail. But if you have a LimitRange enforced with default memory and CPU requests that could be avoided.

Examples:
- <details><summary>Example_1: Setting up resources requests for POD:</summary>

  Let's create a new POD with `res-req-pod` name with `nginx` image:
  ```shell
  k run res-req-pod --image=nginx --dry-run=client -o yaml > res-req-pod.yaml
  ```

  Open the `` file and set up requests as `128Mi` memory and as `128m` cpu:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: res-req-pod
    name: res-req-pod
  spec:
    containers:
    - image: nginx
      name: res-req-pod
      resources:
        requests:
          memory: "128Mi"
          cpu: "128m"
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  NOTE: By your needs, you can set `limits` as well!

  Checking pod:
  ```shell
  k get po res-req-pod
  ```

  Or, if you have labels:
  ```shell
  k get po -l env=dev
  kubectl get pods --selector env=dev
  ```

</details>

- <details><summary>Example_2: Setting up resources limits for POD:</summary>

  Let's create a new POD with `res-lim-pod` name with `nginx` image:
  ```shell
  k run res-lim-pod --image=nginx --dry-run=client -o yaml > res-lim-pod.yaml
  ```

  Open the `res-lim-pod.yaml`file and set up limits as `128Mi` memory and as `128m` cpu:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: res-lim-pod
    name: res-lim-pod
  spec:
    containers:
    - image: nginx
      name: res-lim-pod
      resources:
        limits:
          memory: "128Mi"
          cpu: "128m"
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  NOTE: By your needs, you can set `requests` as well!

  Checking pod:
  ```shell
  k get po res-lim-pod
  ```

</details>

- <details><summary>Example_3: Setting up ResourceQuota for POD:</summary>

  When several users or teams share a cluster with a fixed number of nodes, there is a concern that one team could use more than its fair share of resources. Resource quotas are a tool for administrators to address this concern. A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that namespace.

  Resource quotas work like this:
  - Different teams work in different namespaces. This can be enforced with RBAC.
  - The administrator creates one ResourceQuota for each namespace.
  - Users create resources (pods, services, etc.) in the namespace, and the quota system tracks usage to ensure it does not exceed hard resource limits defined in a ResourceQuota.
  - If creating or updating a resource violates a quota constraint, the request will fail with HTTP status code 403 FORBIDDEN with a message explaining the constraint that would have been violated.
  - If quota is enabled in a namespace for compute resources like cpu and memory, users must specify requests or limits for those values; otherwise, the quota system may reject pod creation. Hint: Use the LimitRanger admission controller to force defaults for pods that make no compute resource requirements.

  To play around with quotas, let's create namespace with `quotas-ns` name:
  ```shell
  k create ns quotas-ns
  ```

  Create `ResourceQuota` for `quotas-ns` namespace:
  ```shell
  k create quota -n quotas-ns my-rq --hard=cpu=1,memory=1G,pods=2 --dry-run=client -o yaml
  ```

  Or, you can use the next example:
  ```shell
  k create quota my-rq -n quotas-ns --hard=requests.cpu=1,requests.memory=1Gi,limits.cpu=2,limits.memory=2Gi
  ```

  Or, let's create quotas with `quotas-ns` name for that created NS (manual way). Open `quotas-ns.yaml` file and put:
  ```yaml
  apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: quotas-ns
    namespace: quotas-ns
  spec:
    hard:
      requests.cpu: "1"
      requests.memory: 1Gi
      limits.cpu: "2"
      limits.memory: 2Gi
  ```

  Apply the file:
  ```shell
  k apply -f quotas-ns.yaml
  ```

  Now, create POD/deploy to verify how it working:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: quota-demo
    namespace: quotas-ns
  spec:
    containers:
    - name: quota-demo-ctr
      image: nginx
      resources:
        limits:
          memory: "800Mi"
          cpu: "800m"
        requests:
          memory: "600Mi"
          cpu: "400m"
  ```

  Apply it:
  ```shell
  k apply -f quota-demo.yaml
  ```

  To check quota stats:
  ```shell
  k get resourcequotas -n quotas-ns -o yaml
  ```

  Or, one more example:
  ```shell
  k get resourcequotas quotas-ns -n quotas-ns  -o jsonpath='{ .status.used }' | jq -r '.'
  k get resourcequotas quotas-ns -n quotas-ns  -o jsonpath='{ .status.hard }' | jq -r '.'
  ```

</details>

- <details><summary>Example_4: Setting up LimitRange for POD:</summary>

  By default, containers run with unbounded compute resources on a Kubernetes cluster. Using Kubernetes resource quotas, administrators (also termed cluster operators) can restrict consumption and creation of cluster resources (such as CPU time, memory, and persistent storage) within a specified namespace. Within a namespace, a Pod can consume as much CPU and memory as is allowed by the ResourceQuotas that apply to that namespace. As a cluster operator, or as a namespace-level administrator, you might also be concerned about making sure that a single object cannot monopolize all available resources within a namespace.

  A LimitRange is a policy to constrain the resource allocations (limits and requests) that you can specify for each applicable object kind (such as Pod or PersistentVolumeClaim) in a namespace.

  A LimitRange provides constraints that can:
  - Enforce minimum and maximum compute resources usage per Pod or Container in a namespace.
  - Enforce minimum and maximum storage request per PersistentVolumeClaim in a namespace.
  - Enforce a ratio between request and limit for a resource in a namespace.
  - Set default request/limit for compute resources in a namespace and automatically inject them to Containers at runtime.

  A LimitRange is enforced in a particular namespace when there is a LimitRange object in that namespace.

  Constraints on resource limits and requests:
  - The administrator creates a LimitRange in a namespace.
  - Users create (or try to create) objects in that namespace, such as Pods or PersistentVolumeClaims.
  - First, the LimitRange admission controller applies default request and limit values for all Pods (and their containers) that do not set compute resource requirements.
  - Second, the LimitRange tracks usage to ensure it does not exceed resource minimum, maximum and ratio defined in any LimitRange present in the namespace.
  - If you attempt to create or update an object (Pod or PersistentVolumeClaim) that violates a LimitRange constraint, your request to the API server will fail with an HTTP status code 403 Forbidden and a message explaining the constraint that has been violated.
  - If you add a LimitRange in a namespace that applies to compute-related resources such as cpu and memory, you must specify requests or limits for those values. Otherwise, the system may reject Pod creation. LimitRange validations occur only at Pod admission stage, not on running Pods. 7. If you add or modify a LimitRange, the Pods that already exist in that namespace continue unchanged.
  - If two or more LimitRange objects exist in the namespace, it is not deterministic which default value will be applied.

  Create a new NS:
  ```shell
  kubectl create ns limitrange
  ```

  Next, create a file with `ns-memory-limit.yaml` name and put the next content:
  ```yaml
  apiVersion: v1
  kind: LimitRange
  metadata:
    name: ns-memory-limit
    namespace: limitrange
  spec:
    limits:
    - max:  max and min define the limit range
        memory: "500Mi"
        cpu: "200m"
      min:
        memory: "100Mi"
        cpu: "200m"
      type: Container
  ```

  Apply the file:
  ```shell
  kubectl apply -f ns-memory-limit.yaml
  ```

  Describe `limitrange`:
  ```shell
  k describe limitrange ns-memory-limit -n limitrange
  ```

  Create an nginx pod that requests 250Mi of memory in the limitrange namespace:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: nginx
    name: nginx
    namespace: limitrange
  spec:
    containers:
    - image: nginx
      name: nginx
      resources:
        requests:
          memory: "250Mi"
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Apply nginx pod:
  ```
  kubectl apply -f nginx.yaml
  ```

</details>

**Useful official documentation**

- [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
- [Resource Quotas in Kubernetes](https://kubernetes.io/docs/concepts/policy/resource-quotas/)

**Useful non-official documentation**

- [A Hands-On Guide to Kubernetes Resource Quotas & Limit Ranges](https://medium.com/@muppedaanvesh/a-hand-on-guide-to-kubernetes-resource-quotas-limit-ranges-%EF%B8%8F-8b9f8cc770c5)

### Understand ConfigMaps

Kubernetes ConfigMap is a built-in Kubernetes API object that’s designed to store your application’s non-sensitive key-value config data. ConfigMaps allow you to keep config values separate from your code and container images. Values can be strings or Base64-encoded binary data.

Once you’ve created a ConfigMap, its content will be saved to your Kubernetes cluster. Pods can then consume the ConfigMap’s values as environment variables, command line arguments, or filesystem volumes.

ConfigMaps are intended to store relatively small amounts of simple data. The total size of a ConfigMap object must be less than 1 MiB. If you need to store more data, you should split your configuration into multiple ConfigMaps or consider using a separate database or key-value store.

It’s good practice to use a ConfigMap to supply any configurable values that your app needs at runtime. The key-value pairs within the ConfigMap can be injected directly into your Pods in several different formats. Because ConfigMaps are part of the Kubernetes API, it’s possible to alter their values without re-deploying your app’s Pods.

Examples:
- <details><summary>Example_1: Create a new configMap:</summary>

  1. Let's create a new configmap with `my-cm` name in `prod` namespace:
    ```shell
    k create cm my-cm -n prod --from-literal=username=username --from-literal=password=MyPasswd --from-literal=host=localhost
    ```

  2. Let's create a new configmap with `my-cm2` name in `prod` namespace:
    ```shell
    echo "username=user" >> creds.txt
    echo "password=password" >> creds.txt
    echo "host=localhost" >> creds.txt
    ```

    NOTE: An example, of `creds.txt` file:
    ```
    username=user
    password=password
    host=localhost
    ```

    Now, create configmap from file:
    ```shell
    k create cm my-cm2 -n prod --from-file=creds=creds.txt
    ```

    To get data, you can use:
    ```shell
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
  ```shell
  k get cm -n prod
  NAME               DATA   AGE
  kube-root-ca.crt   1      11m
  my-cm              3      9m30s
  my-cm2             1      5m59s
  ```

  Create Pod and save it into file:
  ```shell
  k run env-pod-vars -n prod --image=nginx --dry-run=client -o yaml > env-pod-vars.yaml
  ```

  Next, edit `env-pod-vars.yaml` file and create ENV + mount volume:
  
  ```yaml
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
  ```yaml
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

**Let us try to understand the whole concept in following points**:
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


**Limit Kubernetes resources with these top tools**:
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
  ```shell
  k create secret generic new-secret --from-literal=user=username --from-literal=password=password
  ```

  Getting secrets:
  ```shell
  k get secret
  NAME         TYPE     DATA   AGE
  new-secret   Opaque   2      17s
  ```

</details>

- <details><summary>Example_2: Consume secret in Pod as environment variable:</summary>

  Let's create a new pod with `sec-env-var` name to play around with secret and environment variable:
  ```shell
  k run sec-env-var --image=nginx --dry-run=client -o yaml > sec-env-var.yaml
  ```

  Then, open yaml file and attach secret as environemnt variable:
  ```yaml
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
  ```yaml
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
  ```shell
  k apply -f sec-env-var.yaml
  ```

</details>

- <details><summary>Example_3: Consume secret in Pod as volume:</summary>

  Let's create a new pod with `sec-volume-var` name to play around with secret and mount it as volume:
  ```shell
  k run sec-volume-var --image=nginx --dry-run=client -o yaml > sec-volume-var.yaml
  ```

  Then, open yaml file and mount secret as volume. The name you can use by yourself:
  ```yaml
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
  ```yaml
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
  ```shell
  k apply -f sec-volume-var.yaml
  ```

</details>

- <details><summary>Example_4: Get, read, decode secrets:</summary>

  Getting secrets:
  ```shell
  k get secret
  NAME         TYPE     DATA   AGE
  new-secret   Opaque   2      17s
  ```

  Let's read secret:
  ```shell
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
  ```shell
  k get secrets new-secret -ojsonpath='{.data.user}' | base64 -d > user.txt
  k get secrets new-secret -ojsonpath='{.data.password}' | base64 -d > password.txt
  ```

</details>

- <details><summary>Example_5: Encrypting secret data at REST:</summary>

  Creating folder for this task:
  ```shell
  mkdir -p /etc/kubernetes/enc
  ```

  Encrypt secret phrase, for example:
  ```shell
  echo -n Secret-ETCD-Encryption | base64
    U2VjcmV0LUVUQ0QtRW5jcnlwdGlvbg==
  ```

  Create EncryptionConfiguration `/etc/kubernetes/enc/encryption.yaml` file:
  ```yaml
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
  ```yaml
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
  ```shell
  watch crictl ps
  ```

  When `apiserver` will be re-created, we can encrypt all existing secrets. For example, let's do it fort all secrets in `one1` NS:
  ```shell
  kubectl -n one1 get secrets -o json | kubectl replace -f -
  ```

  To check you can do for example:
  ```shell
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
  ```shell
  k get sa -n prod
  ```

  To create a new service account:
  ```shell
  k create sa -n prod my-custom-sa
  ```

  Create clusterrole/role for our needs:
  ```shell
  k create clusterrole my-custom-role --verb create,delete --resource deployments
  ```

  Next, let's add some permissions for SA:
  ```shell
  k -n prod create rolebinding my-custom-rb --clusterrole my-custom-role --serviceaccount prod:my-custom-sa
  ```

  To check, you can use the next command:
  ```shell
  k auth can-i update deployments --as system:serviceaccount:prod:my-custom-sa -n prod
  k auth can-i list pods --as system:serviceaccount:prod:my-custom-sa -n prod
  ```

</details>

- <details><summary>Example_2: Working with SA: need to find witch SA uses for web3 PODs in qa namespace and fix/add list & delete for pods and deployments.</summary>

  Getting SA in NS:
  ```shell
  k get sa -n qa
  NAME           SECRETS   AGE
  default        0         4m24s
  my-custom-sa   0         2m30s
  ```

  Need to find which resource attached into that SA, checking:
  ```shell
  k get rolebindings -n qa -o wide
  NAME           ROLE                AGE     USERS   GROUPS   SERVICEACCOUNTS
  my-custom-rb   Role/list-secrets   5m7s                     qa/my-custom-sa
  smoke          Role/smoke          7m51s   smoke            
  smoke-view     ClusterRole/view    7m45s   smoke
  ```

  Gotcha! It's rolebinding with `my-custom-rb` name. That `rb` uses `list-secrets` role. So, need to edit that role and put proper permissions:
  ```shell
  k edit -n qa role ist-secrets
  ```

  To check, use:
  ```shell
  k auth can-i list deployments --as system:serviceaccount:qa:my-custom-sa -n qa
  k auth can-i delete deployments --as system:serviceaccount:qa:my-custom-sa -n qa
  k auth can-i list po --as system:serviceaccount:qa:my-custom-sa -n qa
  k auth can-i delete po --as system:serviceaccount:qa:my-custom-sa -n qa
  ```

</details>

- <details><summary>Example_3: Using service account with POD/Deployment:</summary>

  Cretae a pod with `ipod` name and with image `nginx` name:
  ```shell
  k run ipod --image=nginx --dry-run=client -o yaml > ipod.yaml
  ```

  Add service account with `i-service-account` name:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: null
    labels:
      run: ipod
    name: ipod
  spec:
    serviceAccountName: i-service-account
    containers:
    - image: nginx
      name: ipod
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Deploy pod:
  ```shell
  k apply -f ipod.yaml
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
  ```shell
  k run sec-con-pod --image=redis --dry-run=client -o yaml > sec-con-pod.yaml
  ```

  Now, let's add some security context into the specific POD only, for example:
  ```yaml
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

  Another way to use `securityContext` under containers. It will be used for all containers:
  ```yaml
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
      runAsNonRoot: true
    containers:
    - image: redis
      name: sec-con-pod
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  status: {}
  ```

  Create POD with:
  ```shell
  k apply -f sec-con-pod.yaml
  ```

  Verification:
  ```shell
  k exec -ti sec-con-pod -- id
  
  uid=1000 gid=1000 groups=1000
  ```

</details>

- <details><summary>Example_2:  Working with Capabilities:</summary>

  Let's create a new POD with image name `redis` and store it into the file to work with:
  ```shell
  k run apa-pod --image=redis --dry-run=client -o yaml > sec-capa-pod.yaml
  ```

  Now, let's add some security context into the POD, for example:
  ```yaml
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
  ```shell
  k apply -f sec-capa-pod.yaml
  ```

  Verification:
  ```shell
  k exec -ti sec-capa-pod -- id
  
  uid=1000 gid=1000 groups=1000
  ```

  And, can check capabilities:
  ```shell
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
- <details><summary>Example_1: Create default deny networking policy with <b>deny-all</b> name in <b>monitoring</b> namespace:</summary>
  
  For example:
  ```yaml
  ---
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: deny-all
    namespace: monitoring
  spec:
    podSelector: {}
    policyTypes:
    - Ingress
    - Egress
  ```

</details>

- <details><summary>Example_2: Create networking policy with <b>api-allow</b> name and create a restriction access to <b>api-allow</b> application that has deployed on <b>default</b> namespace and allow access only from <b>app2</b> pods:</summary>
  
  For example:
  ```yaml
  ---
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadata:
  name: api-allow
  spec:
  podSelector:
    matchLabels:
    run: my-app
  ingress:
  - from:
     - podSelector:
     matchLabels:
       run: app2
  ```

</details>

- <details><summary>Example_3: Define an allow-all policy which overrides the deny all policy on <b>default</b> namespace:</summary>
  
  For example:
  ```yaml
  ---
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: allow-all
    namespace: default
  spec:
    podSelector: {}
    policyTypes:
    - Ingress
    - Egress
    ingress: {}
    egress: {}
  ```

</details>

 - <details><summary>Example_4: Create default deny networking policy for ingress only. Use netpol in <b>monitoring</b> namespace:</summary>

  For example:
  ```yaml
  ---
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: deny-ingress-only
    namespace: monitoring
  spec:
    podSelector: {}
    policyTypes:
    - Ingress
  ```

</details>

- <details><summary>Example_5: Create default deny networking policy for egress only. Use netpol in <b>monitoring</b> namespace:</summary>
  
  For example:
  ```yaml
  ---
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: deny-egress-only
    namespace: monitoring
  spec:
    podSelector: {}
    policyTypes:
    - Egress
  ```

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- None

### Provide and troubleshoot access to applications via services

Examples:
- <details><summary>Example_1: Service with NodePort:</summary>

  Creating a new POD:
  ```shell
  k run redis --image=redis:alpine -ltier=db
  ```

  Now, create a `redis-service` service to expose the redis application within the cluster on port `6379`:
  ```shell
  k expose pod redis --port=6379 --name redis-service
  ```

  Also, you can create service by `kubectl` command:
  ```shell
  k create service nodeport redis --tcp=6379:6379 --node-port=30008 --dry-run=client -o yaml
  ```

</details>

- <details><summary>Example_2: Service with ClusterIP:</summary>

  Create a pod called `httpd` using the `httpd:alpine` image in the default namespace. Next, create a `service` of type `ClusterIP` by the same name (httpd). The target port for the service should be `80`:
  ```shell
  kubectl run httpd --image=httpd:alpine --port=80 --expose
  ```

</details>

- <details><summary>Example_3: Service with LoadBalancer:</summary>

  Creating a new POD:
  ```shell
  k run redis --image=redis:alpine -ltier=db
  ```

  Generate svc template:
  ```shell
  k expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml > redis-service.yaml
  ```

  Edit file with svc and change:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    creationTimestamp: null
    labels:
      tier: db
    name: redis-service
  spec:
    ports:
    - port: 6379
      protocol: TCP
      targetPort: 6379
    selector:
      tier: db
    clusterIP: 10.43.0.18
    type: LoadBalancer
  status:
    loadBalancer:
      ingress:
      - ip: 192.0.2.127
  ```

  Also, you can create service by `kubectl` command:
  ```shell
  k create service nodeport redis-service --tcp=6379:6379 --node-port=30009 --dry-run=client -o yaml
  ```

</details>

**Useful official documentation**

- [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/)

**Useful non-official documentation**

- None

### Use Ingress rules to expose applications

Examples:
- <details><summary>Example_1: Create Ingress with Nginx:</summary>

  Get ingress in `app-space` namespace:
  ```shell
  k get ingress -n app-space
  ```

  Congig file:
  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /
      nginx.ingress.kubernetes.io/ssl-redirect: "false"
    name: ingress-wear-watch
    namespace: app-space
  spec:
    rules:
    - http:
        paths:
        - backend:
            service:
              name: wear-service
              port: 
                number: 8080
          path: /wear
          pathType: Prefix
        - backend:
            service:
              name: video-service
              port: 
                number: 8080
          path: /stream
          pathType: Prefix
  ```

   Apply it:
   ```shell
   k apply -f ingress-wear-watch.yaml
   ```

  Also, you can generate ingress through CLI, let's get another example:
  ```shell
  k create ingress ingress-app1 --class=nginx --rule="*/*=app1-svc:80" --annotation="nginx.ingress.kubernetes.io/rewrite-target=/" --dry-run=client -o yaml > ingress-app1.yaml
  ```

  Apply the config:
  ```shell
  k apply -f ingress-app1.yaml
  ```

</details>

- <details><summary>Example_2: Create ingress with <b>ingress-app1</b> name in <b>app1</b> namespace (with TLS):</summary>
  
  Example:
  ```yaml
  ---
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
  name: ingress-app1
  namespace: app1
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
  spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - "local.domail.name"
    secretName: local-domain-tls
  rules:
  - http:
    paths:
    - path: /health
      pathType: Prefix
      backend:
      service:
        name: app1-svc
        port:
        number: 80
  ```

  **NOTE:** You should create the needed <b>local-domain-tls</b> secret for Ingress with certifications:
  
  ```shell
  openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.crt -subj "/CN=local.domail.name/O=local.domail.name"
  kubectl -n app1 create secret tls local-domain-tls --key cert.key --cert cert.crt
  ```

</details>

- <details><summary>Example_3: Create Ingress with Traefik:</summary>

  Create a new NS:
  ```shell
  kubectl create namespace traefik
  ```

  Add repo:
  ```shell
  helm repo add traefik https://helm.traefik.io/traefik
  helm repo update
  ```

  You can use the following command to search for the latest available version of the Traefik Helm chart:
  ```shell
  helm search repo traefik/traefik --versions
  ```

  Install Traefik ingress controller in Kubernetes:
  ```shell
  helm install traefik  \
    -n traefik \
    --set dashboard.enabled=true \
    traefik/traefik
  ```

  Exposing the Traefik dashboard:
  ```shell
  kubectl port-forward -n traefik $(kubectl get pods -n traefik --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
  ```

  An example of configuration (Deployment, Service and Ingress):
  ```yaml
  ---
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-custom-container
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: nginx
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
          - name: my-container
            imagePullPolicy: Always
            image: ravindrasingh6969/my-app:latest
            ports:
              - containerPort: 80
  
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: app-service
    labels:
      app: nginx
  spec:
    type: ClusterIP
    selector:
      app: nginx
    ports:
      - protocol: TCP
        port: 80
        targetPort: 80
  
  ---
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: foo
    annotations:
      kubernetes.io/ingress.class: traefik
      traefik.ingress.kubernetes.io/rule-type: PathPrefixStrip
      nginx.ingress.kubernetes.io/ssl-redirect: "false"
  spec:
    ingressClassName: traefik
    rules:
    - host: traefik.captainua.local
      http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: app-service
              port:
                number: 80
  ```
  
  Exposing the service:
  ```shell
  k port-forward -n default services/app-service --address=0.0.0.0 8888:80
  ```
  
  Open browser on `localhost:8888`

</details>

**Useful official documentation**

- None

**Useful non-official documentation**

- [Ingress with Traefik](https://kubernetes-tutorial.schoolofdevops.com/ingress/)
- [kubernetes ingress with Traefik](https://doc.traefik.io/traefik/routing/providers/kubernetes-ingress/annotations)


# Additional useful material

## Articles

1. [Cheatsheet for Kubernetes](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
2. [Handbook](https://www.cncf.io/certification/candidate-handbook)
3. [Exam Tips](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)


## Books

1. [Some useful books has stored in folder](https://github.com/SebastianUA/Certified-Kubernetes-Application-Developer/tree/main/hands-on/books)

## Videos

1. [Kubernetes Certified Application Developer (CKAD) with Tests by Mumshad Mannambeth](https://www.udemy.com/course/certified-kubernetes-application-developer)
2. [CKAD - Learn Concepts and Practice the Certified Kubernetes Application Developer (CKAD) Exam with Hand-On Labs](https://www.udemy.com/course/certified-kubernetes-application-developer-training)

## Containers and Kubernetes Application Developer

1. [Killer.sh CKAD practice exam](https://killer.sh/ckad)
2. [Killer Shell CKAD - Interactive Scenarios for Kubernetes Application Developers](https://killercoda.com/killer-shell-ckad)
3. [Kodekloud playground](https://uklabs.kodekloud.com/courses/labs-certified-kubernetes-application-developer)


# Authors

Created and maintained by:
- [Vitalii Natarov](https://github.com/SebastianUA). An email: [vitaliy.natarov@yahoo.com](vitaliy.natarov@yahoo.com).
- Thanks Petro Diachenko [mehanic2000@gmail.com](mehanic2000@gmail.com) for helps in that materials.


# License
Apache 2 Licensed. See [LICENSE](https://github.com/SebastianUA/Certified-Kubernetes-Application-Developer/blob/main/LICENSE) for full details.