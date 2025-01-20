# Zadania 4

**Instalacja Docker Compose na Linuxie:**
  ```bash
  sudo curl -L "https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  sudo chmod +x /usr/local/bin/docker-compose
  docker-compose --version
  ```

1. Utwórz pliki `docker-compose.yml`, `Dockerfile`, `app.py` oraz `requirements.txt` zgodnie z powyższym przykładem.
2. Zbuduj obraz Docker za pomocą Docker Compose.
3. Uruchom aplikację i bazę danych za pomocą Docker Compose.
4. Zweryfikuj działanie aplikacji pod adresem `http://localhost:5000`.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Utwórz plik `docker-compose.yml`:**

   ```yaml
   version: '3.8'
   services:
     web:
       build:
         context: .
         dockerfile: Dockerfile
       ports:
         - "5000:5000"

     redis:
       image: redis:alpine
   ```

2. **Utwórz plik `Dockerfile`:**

   ```Dockerfile
   FROM python:3.9
   WORKDIR /app
   COPY requirements.txt requirements.txt
   RUN pip install --no-cache-dir -r requirements.txt
   COPY . .
   CMD ["python", "app.py"]
   ```

3. **Utwórz plik `app.py`:**

   ```python
   import redis
   from flask import Flask

   app = Flask(__name__)
   redis_client = redis.StrictRedis(host='redis', port=6379, decode_responses=True)

   @app.route("/")
   def index():
       count = redis_client.incr("hits")
       return f"Hello, Docker Compose! This page has been viewed {count} times."

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=5000)
   ```

4. **Utwórz plik `requirements.txt`:**

   ```
   flask
   redis
   ```

5. **Uruchom Docker Compose:**

   ```bash
   docker-compose up --build
   ```

6. **Sprawdź działanie aplikacji:**
   - Otwórz przeglądarkę i przejdź do `http://localhost:5000`.
   - Aplikacja powinna wyświetlać licznik odwiedzin.

7. **Zatrzymaj kontenery:**

   ```bash
   docker-compose down
   ```
   
</details>