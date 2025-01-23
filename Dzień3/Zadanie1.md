# Zadanie 1

**Przykłady manifestów:**

- **Daemon Set:**
  ```yaml
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
    name: hello-app
    spec:
    selector:
        matchLabels:
        app: hello-app
    template:
        metadata:
        labels:
            app: hello-app
        spec:
        containers:
        - name: hello-app
            image: us-west1-docker.pkg.dev/my-project/hello-repo/hello-app
            ports:
            - containerPort: 8080
  ```

- **Stateful Set:**
  ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
    name: mongo
    spec:
    serviceName: "mongo"
    replicas: 3
    selector:
        matchLabels:
        app: mongo
    template:
        metadata:
        labels:
            app: mongo
        spec:
        containers:
        - name: mongo
            image: mongo:latest
            ports:
            - containerPort: 27017
            volumeMounts:
            - name: mongo-data
            mountPath: /data/db
    volumeClaimTemplates:
    - metadata:
        name: mongo-data
        spec:
        accessModes: ["ReadWriteOnce"]
        resources:
            requests:
            storage: 10Gi
    ```

- **Przykład Operatora (RabbitMQ):**
  ```yaml
  # rabbitmq-cluster.yaml
    apiVersion: rabbitmq.com/v1beta1
    kind: RabbitmqCluster
    metadata:
    name: my-rabbitmq
    namespace: rabbitmq
    spec:
    replicas: 3 # Liczba replik w klastrze
    resources:
        requests:
        memory: 256Mi
        cpu: 250m
        limits:
        memory: 512Mi
        cpu: 500m
    storage:
        storageClassName: standard # Zmień na odpowiednią klasę Storage
        volumeSize: 1Gi
  ```

1. Utwórz DaemonSet uruchamiający `us-west1-docker.pkg.dev/my-project/hello-repo/hello-app` na wszystkich węzłach klastra.
- <details>
  <summary>Instrukcja krok po kroku</summary>

   - Zapisz manifest StatefulSet w pliku `daemonset.yaml`.
   - Zastosuj manifest:
     ```bash
     kubectl apply -f daemonset.yaml
     ```
   - Zweryfikuj działanie:
     ```bash
     kubectl get pods -o wide
     ```


</details>

2. Utwórz StatefulSet zarządzający bazą danych MongoDB.
- <details>
  <summary>Instrukcja krok po kroku</summary>

   - Zapisz manifest StatefulSet w pliku `statefulset.yaml`.
   - Zastosuj manifest:
     ```bash
     kubectl apply -f statefulset.yaml
     ```
   - Sprawdź działanie StatefulSet:
     ```bash
     kubectl get statefulsets
     ```

</details>

3. Wdróż RabbitMQ Operator i skonfiguruj cluster.
- <details>
  <summary>Instrukcja krok po kroku</summary>

    - Zainstaluj helm'a
        ```bash
        curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
        ```
    - Dodaj repo RabbitMQ
        ```bash
        helm repo add vmware-tanzu https://vmware-tanzu.github.io helm-charts 
        helm repo update
        ```

    - Instalacja operatora
        ```bash
        kubectl create namespace rabbitmq-system
        helm install rabbitmq-operator vmware-tanzu/rabbitmq-operator --namespace rabbitmq-system
        ```

    - Zapisz manifest operatora w pliku `operator.yaml`

    - Zastosuj manifest:
     ```bash
     kubectl apply -f operator.yaml
     ```


</details>