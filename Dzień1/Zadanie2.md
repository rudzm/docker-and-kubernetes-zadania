# Zadanie 2

1. Utwórz kontener na bazie obrazu `nginx`.
<details>
  <summary>Instrukcja krok po kroku</summary>

**Tworzenie kontenera:**

   ```bash
   docker create --name my-nginx nginx
   ```

   - Tworzy kontener o nazwie `my-nginx` na bazie obrazu `nginx`.

</details>

2. Uruchom, zatrzymaj, pauzuj, wznów, zabij i usuń kontener, obserwując jego cykl życia.

<details>
  <summary>Instrukcja krok po kroku</summary>

**Uruchamianie kontenera:**

   ```bash
   docker start my-nginx
   ```

   - Uruchamia utworzony kontener.

**Sprawdzenie działania kontenera:**

   ```bash
   docker ps
   ```

   - Wyświetla listę uruchomionych kontenerów.

**Pauzowanie kontenera:**

   ```bash
   docker pause my-nginx
   ```

   - Wstrzymuje działanie wszystkich procesów w kontenerze.

**Wznawianie kontenera:**

   ```bash
   docker unpause my-nginx
   ```

   - Wznawia działanie procesów wstrzymanych przez `docker pause`.

**Zatrzymanie kontenera:**

   ```bash
   docker stop my-nginx
   ```

   - Zatrzymuje działający kontener.

**Zabijanie kontenera:**

   ```bash
   docker kill my-nginx
   ```

   - Natychmiastowo przerywa działanie kontenera.

**Usunięcie kontenera:**

   ```bash
   docker rm my-nginx
   ```

   - Usuwa kontener z systemu.

</details>
