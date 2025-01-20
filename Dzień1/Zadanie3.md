# Zadania 3

**Przykład `Dockerfile`:**

```Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y nginx
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]
```

**Przykład `index.html`:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <title>Welcome to nginx!</title>
</head>
<body>
 <h1>Welcome to nginx!</h1>
 <p>This is a simple HTML page served by nginx in a Docker container.</p>
</body>
</html>
```

**Przykład `Dockerfile` z Multistage Building:**

```Dockerfile
# Pierwszy etap: Budowanie aplikacji
FROM golang:1.18 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Drugi etap: Minimalny obraz do uruchomienia aplikacji
FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/myapp .
ENTRYPOINT ["./myapp"]
```

**Zadania praktyczne:**

- Utwórz obraz Docker na podstawie pliku `Dockerfile` z tagiem `my-ngix`.
- Uruchom kontener z utworzonego obrazu i sprawdź jego działanie.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Utwórz plik `Dockerfile`:**
   - Skopiuj powyższy przykład do pliku o nazwie `Dockerfile`.

2. **Budowanie obrazu:**

   ```bash
   docker build -t my-nginx .
   ```

   - Parametr `-t` pozwala nazwać obraz jako `my-nginx`.

3. **Sprawdzenie zbudowanego obrazu:**

   ```bash
   docker images
   ```

   - Wyświetla listę dostępnych obrazów.

4. **Uruchomienie kontenera:**

   ```bash
   docker run -d -p 8080:80 my-nginx
   ```

   - Uruchamia kontener w tle (`-d`) i mapuje port 8080 hosta na port 80 kontenera.

5. **Weryfikacja działania:**
   - Otwórz przeglądarkę i przejdź do adresu `http://localhost:8080`, aby zobaczyć serwowaną stronę.

</details>

- Utwórz obraz z użyciem `ENTRYPOINT`, który pozwoli na przekazywanie poleceń bashowych podczas uruchamiania kontenera.
- Uruchom kontener i przekaż polecenie `echo "Hello from Docker!"`.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Utwórz plik `Dockerfile` dla dodatkowego zadania:**

```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["/bin/bash", "-c"]
```

2. **Zbuduj obraz:**

```bash
docker build -t my-bash-image .
```

3. **Uruchom kontener z przekazaniem polecenia:**

```bash
docker run my-bash-image "echo \"Hello from Docker!\""
```

4. **Zweryfikuj działanie:**

- Powinieneś zobaczyć w terminalu komunikat: 

```bash
Hello from Docker!
```

</details>



- Utwórz obraz Docker wykorzystujący multistage building do budowy i uruchamiania aplikacji Python.
- Końcowy obraz powinien zawierać tylko środowisko wykonawcze i aplikację.

**Przykładowy kod Python:**

Utwórz plik `app.py` z poniższym kodem:

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
 return "Hello, Docker with Multistage Building!"

if __name__ == "__main__":
 app.run(host="0.0.0.0", port=5000)
```

Dodaj plik `requirements.txt`:

```
flask
```

<details>

<summary>Instrukcja krok po kroku</summary>

1. **Utwórz plik `Dockerfile`:**

```Dockerfile
# Pierwszy etap: Budowanie aplikacji
FROM python:3.9-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

# Drugi etap: Minimalny obraz do uruchomienia aplikacji
FROM python:3.9-alpine
WORKDIR /app
COPY --from=builder /app /app
CMD ["python", "app.py"]
```

2. **Budowanie obrazu:**

```bash
docker build -t my-python-app .
```

3. **Uruchomienie kontenera:**

```bash
docker run -it --rm my-python-app
```

</details>