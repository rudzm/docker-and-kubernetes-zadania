# Zadanie 8

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