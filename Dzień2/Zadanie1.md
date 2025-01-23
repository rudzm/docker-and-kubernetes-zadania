# Zadanie 1

1. Zainstaluj Kind (Kubernetes in Docker) na swojej maszynie lokalnej oraz kubectl.

- <details>
    <summary>Instrukcja krok po kroku</summary>

  - Zainstaluj Kind:
    ```bash
    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind
    ```

  - Zainstaluj Kubectl:
    ```bash
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

</details>

2. Utwórz prosty klaster Kubernetes za pomocą Kind.

- <details>
    <summary>Instrukcja krok po kroku</summary>

  - Utwórz plik configuracyjny `cluster.config`
    ```yaml
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    networking:
      disableDefaultCNI: false
    nodes:
      - role: control-plane 
      - role: worker 
      - role: worker
      - role: worker
    ```

  - Utwórz klaster Kubernetes:
    ```bash
    kind create cluster
    ```

  - Zweryfikuj poprawność konfiguracji:
    ```bash
    kubectl get nodes
    ```


</details>