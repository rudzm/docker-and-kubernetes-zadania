# Zadania 5

1. Uruchom lokalne repozytorium obrazów.
2. Wypchnij obraz do lokalnego repozytorium i pobierz go na inną maszynę.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Uruchomienie lokalnego repozytorium:**

   ```bash
   docker run -d -p 5000:5000 --name registry registry:2
   ```

2. **Zbudowanie obrazu i oznaczenie go:**

   ```bash
   docker build -t localhost:5000/my-nginx .
   ```

3. **Wypchnięcie obrazu do lokalnego repozytorium:**

   ```bash
   docker push localhost:5000/my-nginx
   ```

4. **Usunięcie obrazu z maszyny:**

   - Na innej maszynie wykonaj polecenie:

     ```bash
     docker image rm localhost:5000/my-nginx
     ```

5. **Ponowne pobranie obrazu z lokalnego repozytorium:**

   - Na innej maszynie wykonaj polecenie:

     ```bash
     docker pull localhost:5000/my-nginx
     ```

</details>