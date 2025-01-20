# Zadania 6

1. Utwórz własną sieć `bridge`.
2. Uruchom dwa kontenery w tej samej sieci i przetestuj komunikację między nimi.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Tworzenie sieci:**

   ```bash
   docker network create my-network
   ```

   - Tworzy nową sieć typu `bridge` o nazwie `my-network`.

2. **Uruchomienie kontenerów w tej samej sieci:**

   ```bash
   docker run -d --network my-network --name nginx-container nginx
   docker run -it --network my-network --name busybox-container busybox sh
   ```

   - Pierwszy kontener uruchamia serwer `nginx`, drugi to interaktywna instancja BusyBox.

3. **Test komunikacji:**
   - W kontenerze `busybox-container` wykonaj polecenie:

     ```bash
     wget -qO- http://nginx-container
     ```

   - Powinieneś zobaczyć domyślną stronę startową Nginx.

</details>