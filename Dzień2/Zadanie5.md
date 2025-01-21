# Zadanie 5

1. Zainstaluj Ingress Controller w klastrze.
- <details>
  <summary>Instrukcja krok po kroku</summary>

    - Zainstaluj Ingress Controller (np. NGINX):
    ```bash
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
    ```

    - Sprawdź status kontrolera:
    ```bash
    kubectl get pods -n ingress-nginx
    ```

    - Zweryfikuj dostępność usług Ingress:
    ```bash
    kubectl get svc -n ingress-nginx
    ```
</details>

2. Utwórz Deployment, Service oraz Ingress dla aplikacji „Hello World”.
- <details>
  <summary>Instrukcja krok po kroku</summary>

    - Utwórz plik `deployment.yaml`:
        ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: hello-world
        spec:
            replicas: 2
            selector:
            matchLabels:
                app: hello-world
            template:
            metadata:
                labels:
                app: hello-world
            spec:
                containers:
                - name: hello-world
                image: hashicorp/http-echo
                args:
                - "-text=Hello, World!"
        ```

        ```bash
        kubectl apply -f deployment.yaml
        ```

    - Utwórz plik `service.yaml`:
        ```yaml
        apiVersion: v1
        kind: Service
        metadata:
            name: hello-world-service
        spec:
            selector:
                app: hello-world
            ports:
            - protocol: TCP
                port: 80
                targetPort: 5678
        ```

        ```bash
        kubectl apply -f service.yaml
        ```

    - Utwórz plik `ingress.yaml`:
        ```yaml
        apiVersion: networking.k8s.io/v1
        kind: Ingress
        metadata:
            name: hello-world-ingress
        spec:
            rules:
            - host: hello.local
            http:
                paths:
                - path: /
                pathType: Prefix
                backend:
                    service:
                    name: hello-world-service
                    port:
                        number: 80
        ```

        ```bash
        kubectl apply -f ingress.yaml
        ```
</details>
3. Zweryfikuj działanie aplikacji poprzez przeglądarkę lub `curl`.

- <details>
  <summary>Instrukcja krok po kroku</summary>


  - Zaktualizuj `/etc/hosts` na maszynie lokalnej, dodaj wpis, aby zmapować `hello.local` na adres IP Load Balancera:
    ```bash
    echo "127.0.0.1 hello.local" | sudo tee -a /etc/hosts
    ```

  - Zweryfikuj działanie Ingress:
    - Otwórz przeglądarkę i przejdź do `http://hello.local`.
    - Lub użyj `curl`:
        ```bash
        curl http://hello.local
        ```
</details>
