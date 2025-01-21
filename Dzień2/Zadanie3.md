# Zadanie 3

1. Utwórz Pod przy użyciu manifestu i sprawdź jego status za pomocą `kubectl get pods`.

- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Utwórz plik `pod.yaml` z poniższym manifestem:
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: example-pod
       labels:
         app: my-app
     spec:
       containers:
       - name: my-container
         image: nginx:latest
         ports:
         - containerPort: 80
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f pod.yaml
     ```
  - Sprawdź Pod:
     ```bash
     kubectl get pods
     ```

</details>

2. Zaktualizuj Pod, zmieniając obraz w specyfikacji na `nginx:alpine`, i ponownie zastosuj manifest.
- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Zaktualizuj Pod, zmieniając obraz:
     ```bash
     kubectl edit pod example-pod
     ```
  - Zweryfikuj zmianę obrazu:
     ```bash
     kubectl describe pod example-pod
     ```
</details>

3. Utwórz etykiety dla istniejącego Poda i sprawdź je za pomocą `kubectl get pods --show-labels`.
- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Dodaj etykietę do istniejącego Poda:
     ```bash
     kubectl label pod example-pod environment=production
     ```
  - Sprawdź etykiety:
     ```bash
     kubectl get pods --show-labels
     ```
</details>

4. Utwórz Deployment z 3 replikami dla aplikacji `nginx` i sprawdź skalowanie.
- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Utwórz plik `deployment.yaml`:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: example-deployment
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: my-app
       template:
         metadata:
           labels:
             app: my-app
         spec:
           containers:
           - name: my-container
             image: nginx:latest
             ports:
             - containerPort: 80
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f deployment.yaml
     ```
  - Zweryfikuj skalowanie:
     ```bash
     kubectl get deployments
     ```
</details>

5. Skonfiguruj serwis dla Deploymentu w celu udostępnienia aplikacji na porcie 80.
- <details>
  <summary>Instrukcja krok po kroku</summary>
    ```bash
    kubectl expose deployment example-deployment --port 80
</details>

6. Utwórz serwis każdego typu (ClusterIP, NodePort, LoadBalancer, ExternalName) i zweryfikuj ich działanie.
- <details>
  <summary>Instrukcja krok po kroku</summary>

  - ClusterIP
  <details>
  <summary>Instrukcja krok po kroku</summary>
    
    - Utwórz usługę ClusterIP:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: clusterip-service
     spec:
       selector:
         app: my-app
       ports:
       - protocol: TCP
         port: 80
         targetPort: 80
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f clusterip-service.yaml
     ```
  </details>
  
  - NodePort
  <details>
  <summary>Instrukcja krok po kroku</summary>

  - Utwórz usługę NodePort:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: nodeport-service
     spec:
       type: NodePort
       selector:
         app: my-app
       ports:
       - protocol: TCP
         port: 80
         targetPort: 80
         nodePort: 30007
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f nodeport-service.yaml
     ```
  </details>

  - LoadBalancer
  <details>
  <summary>Instrukcja krok po kroku</summary>

  - Utwórz usługę LoadBalancer:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: loadbalancer-service
     spec:
       type: LoadBalancer
       selector:
         app: my-app
       ports:
       - protocol: TCP
         port: 80
         targetPort: 80
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f loadbalancer-service.yaml
     ```
  </details>
  
  - ExternalName
  <details>
  <summary>Instrukcja krok po kroku</summary>
  - Utwórz usługę ExternalName:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: externalname-service
     spec:
       type: ExternalName
       externalName: google.com
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f externalname-service.yaml
     ```
  </details>

</details>

7. Utwórz PersistentVolume i PersistentVolumeClaim oraz przypisz je do poda.

- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Plik `pv.yaml`:
     ```yaml
     apiVersion: v1
     kind: PersistentVolume
     metadata:
       name: nginx-pv
     spec:
       capacity:
         storage: 1Gi
       accessModes:
       - ReadWriteOnce
       hostPath:
         path: /data/nginx
     ```
  - Plik `pvc.yaml`:
     ```yaml
     apiVersion: v1
     kind: PersistentVolumeClaim
     metadata:
       name: nginx-pvc
     spec:
       accessModes:
       - ReadWriteOnce
       resources:
         requests:
           storage: 1Gi
     ```
  - Aplikacja manifestów
    ```bash
    kubectl apply -f pv.yaml
    kubectl apply -f pvc.yaml
    ```

  - Plik `pod-with-pvc.yaml` który wykorzystuje utworzone PVC:
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: nginx-pod-with-pvc
     spec:
       containers:
       - name: nginx
         image: nginx:latest
         volumeMounts:
         - mountPath: "/usr/share/nginx/html"
           name: nginx-storage
       volumes:
       - name: nginx-storage
         persistentVolumeClaim:
           claimName: nginx-pvc
     ```
  - Utworzenie nowego poda
    ```bash
    kubectl apply -f pod-with-pvc.yaml
    ```

</details>

8. Utworzenie sekretu

- <details>
  <summary>Instrukcja krok po kroku</summary>

    - Plik `secret.yaml` który wykorzystamy w podzie:
        ```yaml
        apiVersion: v1
        kind: Secret
        metadata:
            name: mysecret
        type: Opaque
        data:
            password: d3d3MTIz
        ```
    - Utworzenie sekretu:
        ```bash
        kubectl apply -f secret.yaml
        ```

</details>

8. Utworzenie CnfigMapy

- <details>
  <summary>Instrukcja krok po kroku</summary>

    - Plik `configmap.yaml` który wykorzystamy w podzie:
        ```yaml
        apiVersion: v1
        kind: ConfigMap
        metadata:
            name: mycm
        data:
            key: value
            config-file: |
                key: value
                key2: value2
        ```
    - Utworzenie sekretu:
        ```bash
        kubectl apply -f configmap.yaml
        ```

</details>

9. Załadowanie ConfigMap i Secret do poda:

8. Utworzenie CnfigMapy

- <details>
  <summary>Instrukcja krok po kroku</summary>

    - Plik `pod-with-cm-and-secret.yaml` który wykorzystamy w podzie:
        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
            name: nginx-pod-with-configmap
        spec:
            containers:
            - name: nginx
              image: nginx:latest
              envFrom:
              - configMapKeyRef:
                  name: mycm
              - secretKeyRef:
                  name: mysecret
        ```
    - Utworzenie sekretu:
        ```bash
        kubectl apply -f configmap.yaml
        ```

</details>