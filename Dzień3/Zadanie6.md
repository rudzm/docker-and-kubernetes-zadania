# Zadanie 6

1. Skonfiguruj uwierzytelnianie przy użyciu certyfikatu klienta dla użytkownika `developer`.
2. Utwórz Role i RoleBinding umożliwiające użytkownikowi `developer` odczytanie listy podów w przestrzeni nazw `default`.
3. Przetestuj dostęp użytkownika `developer` do zasobów w klastrze.
- <details>
  <summary>Instrukcja krok po kroku</summary>

  - Stwórz klucz prywatny
    ```bash
    openssl genrsa -out myuser.key 2048
    openssl req -new -key myuser.key -out myuser.csr -subj "/CN=myuser"
    ```

  - Stwórz manifest `user.yaml`
    ```yaml
    apiVersion: certificates.k8s.io/v1
    kind: CertificateSigningRequest
    metadata:
      name: myuser
    spec:
      request: fill_up
      signerName: kubernetes.io/kube-apiserver-client
      expirationSeconds: 86400  # one day
      usages:
      - client auth
    ```

  - Wygeneruj klucz dla pola `request`
    ```bash
    cat myuser.csr | base64 | tr -d "\n"
    ```

  - Zakceptuj certyfikat
    ```bash
    kubectl get csr
    kubectl certificate approve myuser
    ```

  - Pobierz podpisany certyfikat
    ```bash
    kubectl get csr myuser -o jsonpath='{.status.certificate}'| base64 -d > myuser.crt
    ```

  - Stwórz `Role` i `RoleBinding`
    ```bash
    kubectl create role developer --verb=create --verb=get --verb=list --verb=update --verb=delete --resource=pods
    kubectl create rolebinding developer-binding-myuser --role=developer --user=myuser
    ```

  - Dodaj konfigurację dla `Kubectl`
    ```bash
    kubectl config set-credentials myuser --client-key=myuser.key --client-certificate=myuser.crt --embed-certs=true
    kubectl config set-context myuser --cluster=nazwa_klastra --user=myuser
    kubectl config use-context myuser
    ```


</details>