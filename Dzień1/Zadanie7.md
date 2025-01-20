1. Stwórz wolumen i zamontuj go w kontenerze.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Tworzenie wolumenu:**

   ```bash
   docker volume create my-volume
   ```

   - Tworzy wolumen o nazwie `my-volume`.

2. **Uruchomienie kontenera z wolumenem:**

   ```bash
   docker run -it -v my-volume:/data busybox sh
   ```

   - Montuje wolumen `my-volume` w katalogu `/data` wewnątrz kontenera.

</details>

2. Przetestuj trwałość danych, zapisując plik w wolumenie i sprawdzając go po usunięciu kontenera.

<details>
    <summary>Instrukcja krok po kroku</summary>

1. **Zapisanie danych w wolumenie:**

   - Wewnątrz kontenera:

     ```bash
     echo "Test data" > /data/test.txt
     ```

2. **Sprawdzenie danych po usunięciu kontenera:**
   - Usuń kontener:

     ```bash
     docker rm -f <container-id>
     ```

   - Uruchom nowy kontener z tym samym wolumenem:

     ```bash
     docker run --rm -v my-volume:/data busybox cat /data/test.txt
     ```

   - Powinieneś zobaczyć wcześniej zapisane dane: `Test data`.

</details>

3. Zamontuj katalog z dysku hosta jako bind mount i przetestuj zapisywanie danych.

<details>
    <summary>Instrukcja krok po kroku</summary>

   - Stwórz katalog na hoście:

     ```bash
     mkdir -p ~/docker-bind-mount
     ```

   - Uruchom kontener z bind mountem:

     ```bash
     docker run -it -v ~/docker-bind-mount:/data busybox sh
     ```
     
   - Wewnątrz kontenera zapisz dane:

     ```bash
     echo "Bind mount test data" > /data/test.txt
     ```

   - Zweryfikuj dane na hoście:

     ```bash
     cat ~/docker-bind-mount/test.txt
     ```

   - Powinieneś zobaczyć: `Bind mount test data`.

</details>