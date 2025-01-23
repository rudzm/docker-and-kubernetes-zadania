# Zadanie 4

1. Utwórz 2 pody i Network Policy który zezwoli na ruch przychodzący tylko z jednego poda
- <details>
  <summary>Instrukcja krok po kroku</summary>

    1. **Utwórz plik `pod-my-app.yaml`:**
        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
            name: pod-my-app
            labels:
            app: my-app
        spec:
            containers:
            - name: nginx
            image: nginx:latest
        ```
        ```bash
        kubectl apply -f pod-my-app.yaml
        ```

    2. **Utwórz plik `pod-frontend.yaml`:**
        ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
            name: pod-frontend
            labels:
            role: frontend
        spec:
            containers:
            - name: busybox
            image: busybox
            command:
                - sleep
                - "3600"
        ```
        ```bash
        kubectl apply -f pod-frontend.yaml
        ```

    3. **Utwórz plik `network-policy.yaml`:**
        ```yaml
        apiVersion: networking.k8s.io/v1
        kind: NetworkPolicy
        metadata:
            name: allow-specific-ingress
            namespace: default
        spec:
            podSelector:
            matchLabels:
                app: my-app
            policyTypes:
            - Ingress
            ingress:
            - from:
            - podSelector:
                matchLabels:
                    role: frontend
            ports:
            - protocol: TCP
                port: 80
        ```
        ```bash
        kubectl apply -f network-policy.yaml
        ```

    4. **Testuj komunikację:**
        - Zaloguj się do `pod-frontend` i spróbuj nawiązać połączenie z `pod-my-app` na porcie 80:
            ```bash
            kubectl exec -it pod-frontend -- wget -qO- http://pod-my-app:80
            ```
        - Spróbuj nawiązać połączenie z innego poda (bez etykiety `role: frontend`) i zweryfikuj, że połączenie jest zablokowane.
</details>