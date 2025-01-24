# Zadanie 8

**Instalacja `Metric Server`**

1. **Pobierz Metric Server**
  ```bash
  wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
  ```

2. Dodaj wpis do konfiguracji
  ```yaml
  spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=10250
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --kubelet-insecure-tls <---- Ten wpis
        - --metric-resolution=15s
        image: registry.k8s.io/metrics-server/metrics-server:v0.7.2

  ```


**Przykład konfiguracji HPA:**

1. **Utwórz Deployment:**

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
   spec:
     selector:
       matchLabels:
         app: nginx
     template:
       metadata:
         labels:
           app: nginx
       spec:
         containers:
         - name: nginx
           image: nginx:latest
           resources:
             requests:
               cpu: "200m"
             limits:
               cpu: "500m"
   ```

2. **Skonfiguruj HPA:**

   ```yaml
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: nginx-hpa
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: nginx-deployment
     minReplicas: 2
     maxReplicas: 10
     metrics:
     - type: Resource
       resource:
         name: cpu
         target:
           type: Utilization
           averageUtilization: 50
   ```


- <details>
  <summary>Instrukcja krok po kroku</summary>

  1. **Utwórz Deployment:**
   - Utwórz plik `nginx-deployment.yaml` zgodnie z powyższym przykładem.
   - Zastosuj plik:
     ```bash
     kubectl apply -f nginx-deployment.yaml
     ```

  2. **Skonfiguruj HPA:**
    - Utwórz plik `nginx-hpa.yaml` zgodnie z powyższym przykładem.
    - Zastosuj plik:
      ```bash
      kubectl apply -f nginx-hpa.yaml
      ```

  3. **Testuj działanie HPA:**
    - Wygeneruj obciążenie:
      ```bash
      kubectl run -i --tty load-generator --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://nginx-deployment.default.svc.cluster.local; done"
      ```
    - Sprawdź skalowanie replik:
      ```bash
      kubectl get hpa
      kubectl get pods
      ```
</details>