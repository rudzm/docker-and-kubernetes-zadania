# Zadanie 4


1. Zainstaluj Prometheus i Grafana oraz uzyskaj do nich dostęp
- <details>
  <summary>Instrukcja krok po kroku</summary>

  1. **Zainstaluj Prometheus i Grafana za pomocą Helm:**

      ```bash
      helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
      helm repo update
      helm install kube-prometheus prometheus-community/kube-prometheus-stack
      ```

  2. **Uzyskaj dostęp do Grafana:**
    - Sprawdź status instalacji:
      ```bash
      kubectl get pods -n default
      ```
    - Uzyskaj dane logowania do Grafana:
      ```bash
      kubectl get secret --namespace default kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
      ```
    - Uruchom tunel do Grafana:
      ```bash
      kubectl port-forward svc/kube-prometheus-stack-grafana 3000:80 -n default
      ```
    - Otwórz przeglądarkę i przejdź do `http://localhost:3000`.
</details>