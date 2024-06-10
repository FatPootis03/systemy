
| Spis treści                              |
| ---------------------------------------- |
| [[#1.Szablon programu w C]]              |
| [[#2.Funkcje wejścia/wyjścia w C]]       |
| [[#3. Generowanie liczb pseudolosowych]] |
| [[#4.Procesy (]]                         |
| [[#5.C Pipe()]]                          |
| [[#6.mkfifo]]                            |
| [[#7. Wątki]]                            |
| [[#8. Mechanizmy IPC]]                   |
| [[#9. Docker]]                           |





## 1.Szablon programu w C:
#szablon_programu_C
```C
#include <stdio.h>

int main()
{
    printf("Hello World!\n");
    return 0;
}
```

---

## 2.Funkcje wejścia/wyjścia w C:
#funkcje_i/o

Funkcje wejścia/wyjścia w konsoli to `printf` i `scanf`. Znajdują się w bibliotece `stdio.h`.

**printf()** - służy do wypisywania sformatowanego tekstu na standardowe wyjście (konsola).

```C
int printf(const char *format, ...);
```

- **`format`**: Łańcuch formatujący, który zawiera tekst do wyświetlenia oraz specyfikatory formatu, które określają, jak mają być formatowane dodatkowe argumenty.
- **`...`**: Dodatkowe argumenty, które są wstawiane w miejsca specyfikatorów formatu.

**Specyfikatory formatu** są wprowadzane przez znak `%` i określają, jak mają być formatowane argumenty:

- `%d` lub `%i`: Liczba całkowita w postaci dziesiętnej.
- `%f`: Liczba zmiennoprzecinkowa.
- `%s`: Łańcuch znaków.
- `%c`: Pojedynczy znak.
- `%x`: Liczba całkowita w postaci szesnastkowej.
- `%o`: Liczba całkowita w postaci ósemkowej.
- `%%`: Znak procentu.


```C
#include <stdio.h>
int main() {
    int age = 25;
    double height = 5.9;
    const char *name = "John Doe";
    
    printf("Name: %s\n", name);
    printf("Age: %d\n", age);
    printf("Height: %.1f\n", height);

    return 0;
}
```

**scanf()** - służy do wczytywania sformatowanych danych ze standardowego wejścia

```C
int scanf(const char *format, ...);
```

- **`format`**: Łańcuch formatujący, który zawiera specyfikatory formatu określające typy danych do wczytania.
- **`...`**: Wskaźniki do zmiennych, do których będą wczytywane dane.

Korzysta z takich samych specyfikatorów jak `printf()`.

```C
#include <stdio.h>

int main() {
    int age;
    double height;
    char name[50];

    printf("Enter your name: ");
    scanf("%s", &name);

    printf("Enter your age: ");
    scanf("%d", &age);

    printf("Enter your height: ");
    scanf("%lf", &height);

    printf("Name: %s\nAge: %d\nHeight: %.1f\n", name, age, height);

    return 0;
}

```
---


## Operacje na plikach w C:

##### Otwieranie plików w C:

```C
#include <stdio.h>
int main(){

FILE *fptr;
// Otwieranie
	fptr = fopen("filename.txt", "w");		 
	//tutaj plik tworzy się tam gdzie nasz plik C (domyślna ścieżka)

	//Jeśli nie istneje taki plik jaki chcemy otworzyć to fopen zwraca wartość NULL

	// Zamykanie pliku
	fclose(fptr); 
}
```

Tworzymy tu wskaźnik typu FILE, który będzie wskazywał na plik, po czym ustawiamy go na żądany plik funkcją `fopen`, oraz wybieramy tryb otwarcia pliku.

**Funkcja fopen()** - jest używana do otwierania plików w różnych trybach w języku C. Jest zdefiniowana w nagłówku `stdio.h`

 Tryby otwarcia pliku:
 
- *r* - otwiera do odczytu <mark style="background: #FF5582A6;">(plik musi istnieć)</mark>
- *w* -otwiera do zapisu <mark style="background: #BBFABBA6;">(gdy go nie ma, to zostaje utworzony)</mark>
- *a* - otwiera plik do dopisywania <mark style="background: #BBFABBA6;">(jeśli nie istnieje, zostanie utworzony)</mark>
- *r+* - otwiera do odczytu i zapisu <mark style="background: #FF5582A6;">(musi istnieć)</mark>
- *w+* - otwiera plik do odczytu i zapisu <mark style="background: #BBFABBA6;">(jeśli plik istnieje, jego zawartość zostanie skasowana; jeśli nie istnieje, zostanie utworzony).</mark>
- *a+* - otwiera plik do odczytu i dopisywania <mark style="background: #BBFABBA6;">(jeśli nie istnieje, zostanie utworzony).</mark>

Funkcja `fopen` zwraca wskaźnik do obiektu typu `FILE`. Jeśli otwarcie pliku się nie powiedzie (np. plik nie istnieje w trybie `"r"`), funkcja zwraca `NULL`.

Żeby utworzyć plik w innym miejscu, trzeba podać ścieżkę absolutną, np:

```C
fptr = fopen("C:\directoryname\filename.txt", "w");
```
---
##### Czytanie z pliku:

```C
FILE *fptr;
	// Otwieramy do czytania
	fptr = fopen("filename.txt", "r");
	// Robimy tablicę aby przechować zawartosc pliku
	char myString[100];
	// Zczytujemy zawartosc pliku do tablicy
	while(fgets(myString, 100, fptr)) {		
	  printf("%s", myString);			
	}

	// Zamykam plik
	fclose(fptr); 
```

Tutaj otwieramy plik w trybie "*r*" - *read*, tworzymy tablicę znaków o długości 100, i za pomocą funkcji `fgets()` sczytujemy zawartość pliku do tej tablicy.

**Funkcja fgets()** - służy do odczytywania ciągów znaków z pliku lub innego strumienia do tablicy znaków. Jest zdefiniowana w nagłówku `stdio.h`.
```c
fgets(char *str, int n, FILE *stream);
```
- **char *str** - wskaźnik do bufora, do którego zostanie zapisany odczytany ciąg znaków.
- **int n** - maksymalna liczba znaków do odczytania (włącznie z kończącym znakiem null).
- **stream**: wskaźnik do strumienia, z którego ma być odczytywany ciąg znaków.

Funkcja `fgets` odczytuje znaki z `stream` i zapisuje je do `str` do momentu:

- napotkania znaku nowej linii (`\n`), który również jest zapisywany do `str`,
- odczytania `n-1` znaków,
- lub napotkania końca pliku (EOF).

Po zakończeniu odczytu, do `str` dodawany jest znak null (`\0`).

Funkcja `fgets` zwraca wskaźnik do `str`, jeśli operacja się powiodła, lub `NULL` w przypadku błędu lub końca pliku.

---
##### Sprawdzenie pliku:

Do sprawdzenia czy plik istnieje możemy wykorzystać wartość zwracaną przez funkcje `fopen()`

```C
// sprawdzam czy plik istnieje (fopen nie zwraca null)
	if(fptr != NULL) {

	  // Czyta zawartość pliku i ją wypisuje w konsoli
	  while(fgets(myString, 100, fptr)) {
	    printf("%s", myString);
	  }
	// Jesli plik nie istnieje
	} else {
	  printf("Not able to open the file.");
	}
```
---

##### Zapis do pliku:

**Funkcja fprintf()** - jest częścią biblioteki standardowej języka C i jest używana do wypisywania sformatowanego tekstu do pliku lub innego strumienia.

```C
int fprintf(FILE *stream, const char *format, ...);
```

- `stream`: wskaźnik do obiektu typu `FILE`, który reprezentuje otwarty plik lub strumień, do którego ma być zapisany tekst.
- `format`: łańcuch formatujący, który może zawierać zwykły tekst oraz specyfikatory formatu, takie jak `%d`, `%s`, `%f`, itp.
- `...`: lista dodatkowych argumentów, które są formatowane i wypisywane zgodnie ze specyfikatorami w łańcuchu formatującym (np. zmienne)

Funkcja `fprintf` zwraca liczbę znaków zapisanych do strumienia. Jeśli wystąpi błąd, zwraca wartość ujemną.

**Przykład działania wraz z innymi funkcjami**

```C
#include <stdio.h>
int main() {
    FILE *file = fopen("output.txt", "w");
    if (file == NULL) {
        perror("Error opening file");
        return 1;
    }
    int age = 25;
    const char *name = "John Doe";
    double height = 5.9;
    // Zapis sformatowanego tekstu do pliku
    fprintf(file, "Name: %s\nAge: %d\nHeight: %.1f\n", name, age, height);
    fclose(file);
    return 0;
}
```

> [!NOTE] Wynik programu
> Name: John Doe
> Age: 25
> Height: 5.9

##### Funkcja fseek()

`fseek` jest funkcją biblioteki standardowej C, która służy do przesuwania wskaźnika pozycji w pliku. Umożliwia precyzyjne ustawienie wskaźnika na określoną pozycję w pliku. Składnia funkcji `fseek` wygląda następująco:

```C
int fseek(FILE *stream, long offset, int whence);
```

- `stream` to wskaźnik do pliku, który wcześniej został otwarty za pomocą `fopen`.
- `offset` to liczba bajtów, o którą chcesz przesunąć wskaźnik.
- `whence` określa punkt odniesienia dla przesunięcia wskaźnika. Może przyjmować jedną z trzech wartości:
    - `SEEK_SET`: Ustawia wskaźnik na `offset` bajtów od początku pliku.
    - `SEEK_CUR`: Ustawia wskaźnik na `offset` bajtów od bieżącej pozycji wskaźnika.
    - `SEEK_END`: Ustawia wskaźnik na `offset` bajtów od końca pliku.

Przykład `fseek()`:

```C
FILE *file = fopen("example.txt", "r");
if (file != NULL) {
    fseek(file, 10, SEEK_SET); // Przesuwa wskaźnik o 10 bajtów od początku pliku
    // Odczyt danych po przesunięciu wskaźnika
    fclose(file);
}
```

##### Funkcja ftell()

Funkcja `ftell` w języku C jest używana do uzyskiwania aktualnej pozycji wskaźnika pliku. Jest to przydatne, gdy chcesz znać bieżącą pozycję w pliku (w bajtach od początku pliku). 

```C
long ftell(FILE *stream);
```

- `stream` to wskaźnik do pliku, który wcześniej został otwarty za pomocą `fopen`.

Funkcja `ftell` zwraca bieżącą pozycję wskaźnika pliku jako wartość typu `long`. Jeśli wystąpi błąd, zwraca `-1L`.

---

## 3. Generowanie liczb pseudolosowych
#generowanie_losowe

Generowanie liczb pseudolosowych w C opiera się zazwyczaj na wbudowanej bibliotece standardowej `stdlib.h`, która zawiera funkcje do generowania.
Najpopularniejszą z tych funkcji jest `rand()`, która generuje pseudolosowe liczby całkowite w zakresie od 0 do RAND_MAX. 

Aby funkcja `rand()` działała poprawnie, należy wcześniej użyć `srand()` do inicjalizacji generatora.

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int main() {
    // Inicjalizacja generatora liczb pseudolosowych
    srand(time(NULL)); // Użyj bieżącego czasu jako ziarna
    // Generowanie i wyświetlanie 5 pseudolosowych liczb
    for (int i = 0; i < 5; i++) {
        int random_number = rand();
        printf("%d\n", random_number);
    }

    return 0;
}
```

Aby ograniczyć `rand()`, używamy modulo `%`
```C
int random = rand() % 5; //od 0 do 4
```


<mark style="background: #ADCCFFA6;">Wzór na rand w przedziale</mark>
```C
int randomNumber = rand() % (maxValue - minValue + 1) + minValue;
```
Gdzie `minValue` to dolna granica przedziału, a `maxValue` to górna granica przedziału.

---

## 4.Procesy :(
#procesy

> [!NOTE] Pid w C
> Process Id (Pid) w języku C musi być przechowywany w zmiennej o typie `pid_t`.
##### Funkcja fork()

Funkcja **fork()** - używana w systemach Unix do tworzenia nowego procesu. Jest to podstawowy mechanizm do tworzenia procesów potomnych.

Kiedy proces wywołuje `fork()`, tworzony jest nowy proces, który jest kopią procesu wywołującego (rodzica).

Biblioteki do korzystania z fork to  `<sys/types.h>` i `<unistd.h>`.

###### Kluczowe cechy `fork()`:

1. **Powielanie procesu**:
    - Nowo utworzony proces potomny jest niemal dokładną kopią procesu rodzica. Oznacza to, że obie wersje procesu (rodzic i potomek) będą miały tę samą pamięć, te same zmienne, etc., ale działają jako odrębne procesy.
2. **Wartość zwracana**:
    - Funkcja `fork()` zwraca różne wartości w procesie rodzica i potomnym:
        - W procesie rodzica `fork()` zwraca PID (Process ID) nowo utworzonego procesu potomnego.
        - W procesie potomnym `fork()` zwraca 0.
        - Jeśli `fork()` nie może utworzyć nowego procesu (np. z powodu braku zasobów), zwraca -1.

###### Przykład działania:

```C
#include <stdio.h>
#include <unistd.h>
int main() {
    pid_t pid;
    // Wywołanie funkcji fork
    pid = fork();
    if (pid < 0) {
        // Wystąpił błąd podczas tworzenia procesu potomnego
        fprintf(stderr, "Fork failed\n");
        return 1;
    } else if (pid == 0) {
        // Kod wykonywany przez proces potomny
        printf("To jest proces potomny. PID: %d, PPID: %d\n", getpid(), getppid());
    } else {
        // Kod wykonywany przez proces rodzica
        printf("To jest proces rodzica. PID: %d, PID potomka: %d\n", getpid(), pid);
    }
    return 0;
}
```

###### Drugi przykład:
```C
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid1, pid2;

    // Pierwszy fork do stworzenia pierwszego procesu potomnego
    pid1 = fork();

    if (pid1 < 0) {
        // Wystąpił błąd podczas tworzenia pierwszego procesu potomnego
        fprintf(stderr, "Fork failed\n");
        return 1;
    } else if (pid1 == 0) {
        // Kod wykonywany przez pierwszy proces potomny
        printf("Pierwszy proces potomny: PID = %d, PPID = %d\n", getpid(), getppid());
        // Kod specyficzny dla pierwszego procesu potomnego
        for (int i = 0; i < 5; i++) {
            printf("Pierwszy proces potomny pracuje...\n");
            sleep(1); // Symulacja pracy
        }
        return 0;
    } else {
        // Drugi fork do stworzenia drugiego procesu potomnego
        pid2 = fork();

        if (pid2 < 0) {
            // Wystąpił błąd podczas tworzenia drugiego procesu potomnego
            fprintf(stderr, "Fork failed\n");
            return 1;
        } else if (pid2 == 0) {
            // Kod wykonywany przez drugi proces potomny
            printf("Drugi proces potomny: PID = %d, PPID = %d\n", getpid(), getppid());
            // Kod specyficzny dla drugiego procesu potomnego
            for (int i = 0; i < 5; i++) {
                printf("Drugi proces potomny pracuje...\n");
                sleep(1); // Symulacja pracy
            }
            return 0;
        } else {
            // Kod wykonywany przez proces główny (rodzicielski)
            printf("Proces główny: PID = %d\n", getpid());

            // Oczekiwanie na zakończenie obu procesów potomnych
            wait(NULL);
            wait(NULL);
            printf("Proces główny kończy działanie.\n");
        }
    }

    return 0;
}

```

##### Obsługa zwracanej wartości:

- Jeśli `pid` jest mniejsze niż 0 (`pid < 0`), oznacza to, że wystąpił błąd przy tworzeniu nowego procesu. W takim przypadku kod drukuje komunikat o błędzie i zwraca `1`.
- Jeśli `pid` wynosi 0 (`pid == 0`), oznacza to, że kod działa w kontekście procesu potomnego. Proces potomny może wykonywać swoją unikalną logikę.
- Jeśli `pid` jest większe od 0 (`pid > 0`), oznacza to, że kod działa w kontekście procesu rodzica. Proces rodzica może również wykonywać swoją logikę, a `pid` w tym przypadku będzie PID procesu potomnego.

Proces rodzica i potomek mają oddzielne przestrzenie adresowe, co oznacza, że zmiany dokonane w zmiennych w jednym procesie nie wpływają na zmienne w drugim procesie.

---
#### Funkcja getpid() i getppid()

Funkcja **getpid()** - używana w systemach Linux do uzyskania identyfikatora bieżącego procesu. Jest to bardzo prosta funkcja, która zwraca wartość typu `pid_t` (typ danych reprezentujący identyfikator procesu). Funkcja ta jest w bibliotece `unistd.h`

```C
#include <stdio.h>
#include <unistd.h>
int main() {
    pid_t pid;
    // Uzyskanie identyfikatora bieżącego procesu
    pid = getpid();
    printf("Identyfikator bieżącego procesu: %d\n", pid);
    return 0;
}
```

Funkcja **getppid()** jest używana w systemach Linux do uzyskania identyfikatora procesu rodzica bieżącego procesu. Jest to prosty sposób na sprawdzenie, który proces utworzył bieżący proces (czyli jego rodzica).

```C
#include <stdio.h>
#include <unistd.h>
int main() {
    pid_t parent_pid;
    // Uzyskanie identyfikatora procesu rodzica
    parent_pid = getppid();
    printf("Identyfikator procesu rodzica: %d\n", parent_pid);
    return 0;
}
```

---
#### 5.C Pipe()

Funkcja `pipe()` jest używana do tworzenia nienazwanego potoku (pipe), który umożliwia komunikację między procesami. Potok składa się z dwóch końców: końca do zapisu i końca do odczytu. Dane zapisane do jednego końca potoku mogą być odczytane z drugiego końca, co umożliwia procesom wymianę danych. Aby użyć `pipe()`, należy dołączyć nagłówek `<unistd.h>`.

```C
int pipe(int pipefd[2]);
```


- **Tworzenie potoku**:
    - `pipe()` tworzy nienazwany potok, który jest reprezentowany przez dwa deskryptory plików: jeden do odczytu i jeden do zapisu.
- **Deskryptory plików**:
    - `pipefd[0]` — Deskryptor pliku używany do odczytu z potoku.
    - `pipefd[1]` — Deskryptor pliku używany do zapisu do potoku.

#### Zwracana wartość
- Zwraca `0`, jeśli potok został utworzony pomyślnie.
- Zwraca `-1`, jeśli wystąpił błąd, i ustawia `errno` na wartość wskazującą przyczynę błędu.

##### Przykład:

```C
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    int pipefd[2];  // Tablica do przechowywania deskryptorów plików
    pid_t pid;
    char write_msg[] = "Hello, world!";
    char read_msg[20];

    // Tworzenie potoku
    if (pipe(pipefd) == -1) {
        perror("pipe");
        return 1;
    }

    // Tworzenie procesu potomnego
    pid = fork();

    if (pid < 0) {
        // Wystąpił błąd podczas tworzenia procesu potomnego
        perror("fork");
        return 1;
    } else if (pid == 0) {
        // Kod wykonywany przez proces potomny

        // Zamknięcie deskryptora do zapisu w procesie potomnym
        close(pipefd[1]);

        // Odczyt danych z potoku
        read(pipefd[0], read_msg, sizeof(read_msg));
        printf("Proces potomny otrzymał: %s\n", read_msg);

        // Zamknięcie deskryptora do odczytu w procesie potomnym
        close(pipefd[0]);
    } else {
        // Kod wykonywany przez proces główny

        // Zamknięcie deskryptora do odczytu w procesie głównym
        close(pipefd[0]);

        // Zapis danych do potoku
        write(pipefd[1], write_msg, strlen(write_msg) + 1);
        printf("Proces główny wysłał: %s\n", write_msg);

        // Zamknięcie deskryptora do zapisu w procesie głównym
        close(pipefd[1]);

        // Oczekiwanie na zakończenie procesu potomnego
        wait(NULL);
    }

    return 0;
}
```


## 6.mkfifo

`mkfifo` jest funkcją w C, która tworzy tzw. nazwane potoki (ang. named pipes). Nazwane potoki pozwalają na komunikację między procesami, podobnie jak zwykłe potoki, ale z tą różnicą, że mają przypisaną nazwę w systemie plików, co umożliwia komunikację między procesami niezależnie od pokrewieństwa.

1. **Nagłówek**: Aby użyć funkcji `mkfifo`, musisz dołączyć nagłówek `sys/stat.h`.
2. Składnia:

	```C
int mkfifo(const char *pathname, mode_t mode);
```

- `pathname`: Ścieżka do pliku FIFO (czyli nazwanego potoku), który ma zostać utworzony.
- `mode`: Uprawnienia do pliku w stylu tych używanych w `chmod`. Na przykład `0666` to pełne uprawnienia do odczytu i zapisu dla wszystkich użytkowników.

3. **Zwracana wartość**:
- Zwraca `0` przy sukcesie.
- Zwraca `-1` przy błędzie i ustawia `errno` wskazując przyczynę błędu.

```C
#include <stdio.h>
#include <sys/stat.h>
#include <errno.h>

int main() {
    const char *fifo_path = "/tmp/myfifo";
    mode_t mode = 0666; // pełne uprawnienia do odczytu i zapisu

    if (mkfifo(fifo_path, mode) == -1) {
        if (errno != EEXIST) {
            perror("mkfifo");
            return 1;
        }
    }

    printf("FIFO %s created.\n", fifo_path);
    return 0;
}
```

**Uwaga**: Jeśli potok o podanej nazwie już istnieje, `mkfifo` zwróci błąd z `errno` ustawionym na `EEXIST`.

### Użycie nazwanego potoku

Po utworzeniu nazwanego potoku, możesz go używać jak plik do komunikacji między procesami. Odczyt i zapis do potoku wykonuje się za pomocą standardowych funkcji plikowych `open`, `read`, `write`, i `close`.

Na przykład, jeden proces może pisać do potoku:

```C
int fd = open("/tmp/myfifo", O_WRONLY);
write(fd, "Hello, world!", 13);
close(fd);
```
A inny proces może czytać z potoku:
```C
int fd = open("/tmp/myfifo", O_RDONLY);
char buffer[128];
int n = read(fd, buffer, sizeof(buffer));
buffer[n] = '\0';
printf("Read: %s\n", buffer);
close(fd);
```

flagi używane podczas otwierania plików za pomocą funkcji `open` w systemach Unix/Linux. Flagi te określają sposób, w jaki plik ma być otwarty. Są zdefiniowane w nagłówku `<fcntl.h>`.

Oto najczęściej używane flagi:

1. **O_RDONLY**: Otwiera plik tylko do odczytu.
2. **O_WRONLY**: Otwiera plik tylko do zapisu.
3. **O_RDWR**: Otwiera plik zarówno do odczytu, jak i zapisu.
4. **O_CREAT**: Jeśli plik nie istnieje, tworzy nowy plik. Wymaga podania dodatkowego argumentu `mode`, który określa uprawnienia nowego pliku.

## 7. Wątki

Wątki (ang. threads) w C są używane do tworzenia równoległych sekcji kodu, co pozwala na wykonywanie wielu zadań jednocześnie w ramach tego samego procesu. Wątki dzielą tę samą przestrzeń adresową, co umożliwia im bezpośrednią współpracę, ale również wymaga synchronizacji, aby uniknąć konfliktów. Znajdują się one w bibliotece `pthread`.

### Tworzenie wątku

Funkcja `pthread_create` jest używana do tworzenia nowych wątków.
```C
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```

- `thread`: wskaźnik na identyfikator nowego wątku.
- `attr`: wskaźnik na atrybuty wątku (można podać `NULL` dla domyślnych atrybutów).
- `start_routine`: funkcja, którą wątek ma wykonywać.
- `arg`: argument przekazywany do funkcji `start_routine`.

### Przykład

```C
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void* print_message(void* arg) {
    char* message = (char*) arg;
    printf("%s\n", message);
    return NULL;
}

int main() {
    pthread_t thread;
    const char* message = "Hello from thread!";
    // Tworzenie wątku
    if (pthread_create(&thread, NULL, print_message, (void*) message)) {
        fprintf(stderr, "Error creating thread\n");
        return 1;
    }
    // Czekanie na zakończenie wątku
    if (pthread_join(thread, NULL)) {
        fprintf(stderr, "Error joining thread\n");
        return 2;
    }
    return 0;
}
```

### Synchronizacja wątków

#### Mutexy

Mutexy (ang. mutexes) są używane do synchronizacji dostępu do zasobów współdzielonych między wątkami.

```C
#include <pthread.h>
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* thread_function(void* arg) {
    pthread_mutex_lock(&mutex);
    // Krytyczna sekcja
    pthread_mutex_unlock(&mutex);
    return NULL;
}
```

### Przykład mutexu

```C
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
int counter = 0;

void* increment_counter(void* arg) {
    for (int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&mutex);
        counter++;
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}
int main() {
    pthread_t thread1, thread2;
    pthread_create(&thread1, NULL, increment_counter, NULL);
    pthread_create(&thread2, NULL, increment_counter, NULL);
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);
    printf("Final counter value: %d\n", counter);
    return 0;
}
```
### Podsumowanie

- **Wątki**: Używane do równoległego wykonywania kodu w ramach tego samego procesu.
- **Biblioteka pthread**: Standardowa biblioteka do zarządzania wątkami w Unix/Linux.
- **Tworzenie wątków**: `pthread_create` tworzy nowe wątki.
- **Synchronizacja**: Mutexy (`pthread_mutex_t`) są używane do synchronizacji dostępu do zasobów współdzielonych.
- **Atrybuty wątków**: Pozwalają na konfigurację wątków (np. `pthread_attr_setdetachstate`).

## 8. Mechanizmy IPC

https://www.geeksforgeeks.org/ipc-using-message-queues/
## 9. Docker

https://docs.docker.com/get-started/02_our_app/

https://docs.docker.com/compose/gettingstarted/

https://docs.docker.com/compose/networking/

https://forums.docker.com/t/static-ip-on-docker-containers/110412/5