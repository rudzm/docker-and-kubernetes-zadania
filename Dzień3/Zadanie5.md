# Zadanie 5


1. Utwórz Service Account o nazwie `test-sa`.
2. Skonfiguruj RBAC, które umożliwi temu SA tylko odczyt podów w przestrzeni nazw `default`.
3. Utwórz pod, który używa Service Account `test-sa`.
4. Zweryfikuj, że RBAC działa poprawnie, próbując uzyskać dostęp do listy podów z poziomu kontenera w podzie.
- <details>
  <summary>Instrukcja krok po kroku</summary>

  1. **Utwórz plik ****`serviceaccount.yaml`****:**

      ```yaml
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: test-sa
        namespace: default
      ```

      ```bash
      kubectl apply -f serviceaccount.yaml
      ```

  2. **Utwórz plik ****`role.yaml`****:**

      ```yaml
      apiVersion: rbac.authorization.k8s.io/v1
      kind: Role
      metadata:
        namespace: default
        name: pod-reader
      rules:
      - apiGroups: [""]
        resources: ["pods"]
        verbs: ["get", "watch", "list"]
      ```

      ```bash
      kubectl apply -f role.yaml
      ```

  3. **Utwórz plik ****`rolebinding.yaml`****:**

      ```yaml
      apiVersion: rbac.authorization.k8s.io/v1
      kind: RoleBinding
      metadata:
        name: read-pods
        namespace: default
      subjects:
      - kind: ServiceAccount
        name: test-sa
        namespace: default
      roleRef:
        kind: Role
        name: pod-reader
        apiGroup: rbac.authorization.k8s.io
      ```

      ```bash
      kubectl apply -f rolebinding.yaml
      ```

  4. **Utwórz plik ****`pod.yaml`****:**

      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: pod-with-sa
        namespace: default
      spec:
        serviceAccountName: test-sa
        containers:
        - name: busybox
          image: busybox
          command: ["sleep", "3600"]
      ```

      ```bash
      kubectl apply -f pod.yaml
      ```

  5. **Przetestuj RBAC z poziomu poda:**

    - Zaloguj się do kontenera:
      ```bash
      kubectl exec -it pod-with-sa -- sh
      ```
    - Spróbuj pobrać listę podów:
      ```bash
      curl -s --header "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" \
      --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
      https://kubernetes.default.svc/api/v1/pods
      ```
    - Zweryfikuj, że dostęp jest ograniczony zgodnie z konfiguracją RBAC.

</details>