
# Zadania 1

1. Zainstaluj Docker na swojej maszynie.

<details>
  <summary>Instrukcja krok po kroku</summary>

- **Instalacja Dockera:**
  1. Zaktualizuj listę pakietów:

     ```bash
     sudo apt update
     ```

  2. Zainstaluj Docker:

     ```bash
     sudo apt install docker.io
     ```

  3. Uruchom i włącz Docker podczas uruchamiania systemu:

     ```bash
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

</details>

2. Uruchom obraz `hello-world` i sprawdź jego działanie.

<details>
  <summary>Instrukcja krok po kroku</summary>

- **Uruchomienie kontenera:**
  1. Uruchom obraz testowy:

     ```bash
     docker run hello-world
     ```
     
  2. Zweryfikuj działające kontenery:

     ```bash
     docker ps -a
     ```
</details>
