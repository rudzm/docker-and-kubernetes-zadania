# Zadanie 4

1. Spradź połączenie pomiędzy 2 rónymi podami
- <details>
<summary>Instrukcja krok po kroku</summary>

 - Utwórz dwa Pody w pliku `pods.yaml`:
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: pod-a
     spec:
       containers:
       - name: busybox
         image: busybox
         command:
         - sleep
         - "3600"
     ---
     apiVersion: v1
     kind: Pod
     metadata:
       name: pod-b
     spec:
       containers:
       - name: busybox
         image: busybox
         command:
         - sleep
         - "3600"
     ```
  - Zastosuj manifest:
     ```bash
     kubectl apply -f pods.yaml
     ```
  - Sprawdz adres IP `pod-b`
    ```bash
     kubectl get pods -o wide
     ```
  - Sprawdź komunikację między Podami:
     ```bash
     kubectl exec -it pod-a -- ping pod-b
     ```
</details>
