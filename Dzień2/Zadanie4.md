# Zadanie 4

1. Zainstaluj nowy CNI plugin
- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Popraw plik configuracyjny `cluster.config`
    ```yaml
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    networking:
      disableDefaultCNI: true
    nodes:
      - role: control-plane 
      - role: worker 
      - role: worker
      - role: worker
    ```
    - Pobierz `cilium`
      ```bash
      CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)
      CLI_ARCH=amd64
      if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
      curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
      sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
      sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
      rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
      ```
    - Zainstaluj `cilium`
      ```bash
      cilium install --version 1.16.6
      ```

</details>


2. Spradź połączenie pomiędzy 2 rónymi podami
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
