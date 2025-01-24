# Zadanie 2

**Przykłady manifestów:**

- **Job:**
  ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: example-job
    spec:
      template:
        metadata:
          name: example-job
        spec:
          containers:
          - name: busybox
            image: busybox:latest
            command: ["/bin/sh", "-c", "echo Hello Kubernetes! && sleep 10"]
          restartPolicy: Never
  ```

- **CronJob:**
  ```yaml
    apiVersion: batch/v1
    kind: CronJob
    metadata:
      name: example-cronjob
    spec:
      schedule: "*/5 * * * *"
      jobTemplate:
        spec:
          template:
            spec:
              containers:
              - name: busybox
                image: busybox:latest
                command: ["/bin/sh", "-c", "echo This task runs every 5 minutes"]
              restartPolicy: Never
  ```

1. Utwórz Job, który wypisuje w logach wiadomość "Hello Kubernetes!" i kończy się po wykonaniu zadania.
- <details>
  <summary>Instrukcja krok po kroku</summary>

   - Zapisz manifest Job w pliku `job.yaml`.
   - Zastosuj manifest:
     ```bash
     kubectl apply -f job.yaml
     ```
   - Sprawdź status zadania:
     ```bash
     kubectl get jobs
     ```
   - Wyświetl logi z Poda:
     ```bash
     kubectl logs <job-pod-name>
     ```
</details>

2. Utwórz CronJob, który wypisuje w logach wiadomość co minutę.
- <details>
  <summary>Instrukcja krok po kroku</summary>

   - Zapisz manifest CronJob w pliku `cronjob.yaml`.
   - Zastosuj manifest:
     ```bash
     kubectl apply -f cronjob.yaml
     ```
   - Zweryfikuj harmonogram:
     ```bash
     kubectl get cronjobs
     ```
   - Sprawdź logi Podów uruchamianych przez CronJob:
     ```bash
     kubectl logs <cronjob-pod-name>
     ```
</details>