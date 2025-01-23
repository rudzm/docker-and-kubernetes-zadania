# Zadanie3

1. Utwórz StorageClass o nazwie `local-storage`, która korzysta z zasobów hosta.
- <details>
  <summary>Instrukcja krok po kroku</summary>

    - **Utwórz plik `storageclass.yaml`:**
        ```yaml
        apiVersion: storage.k8s.io/v1
        kind: StorageClass
        metadata:
            name: local-storage
        provisioner: kubernetes.io/hostpath
        volumeBindingMode: WaitForFirstConsumer
        ```
        ```bash
        kubectl apply -f storageclass.yaml
        ```

</details>

2. Utwórz PersistentVolumeClaim korzystający z tej klasy pamięci masowej.
- <details>
  <summary>Instrukcja krok po kroku</summary>

    - **Utwórz plik `pvc.yaml`:**
        ```yaml
        apiVersion: v1
        kind: PersistentVolumeClaim
        metadata:
            name: local-claim
        spec:
            accessModes:
            - ReadWriteOnce
            resources:
            requests:
                storage: 1Gi
            storageClassName: local-storage
        ```
        ```bash
        kubectl apply -f pvc.yaml
        ```
</details>

3. Uruchom Pod, który korzysta z PVC i zapisuje dane w zamontowanym wolumenie.
- <details>
  <summary>Instrukcja krok po kroku</summary>

    - **Utwórz plik `pod-with-local-storage.yaml`:**
        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
            name: pod-with-local-storage
        spec:
            containers:
            - name: my-container
            image: nginx:latest
            volumeMounts:
            - mountPath: "/data"
                name: local-volume
            volumes:
            - name: local-volume
            persistentVolumeClaim:
                claimName: local-claim
        ```
        ```bash
        kubectl apply -f pod-with-local-storage.yaml
        ```
</details>