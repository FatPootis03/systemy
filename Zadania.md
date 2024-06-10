#### Zadanie 1

1. Stwórz plik, w programie C dodaj jakiś ciąg znaków do tego pliku.
2. Jako rozszerzenie poprzedniego zadania program powinien przyjmować nazwę pliku. Program powinien sprawdzić czy plik istnieje (jeśli nie, rzucić odpowiednim błędem) i zapisać jakąś zawartość do niego. Jeśli plik istnieje to zawartość powinna być dopisywana, a nie nadpisywana.
3. Kolejnym rozszerzeniem jest sprawdzenie jaką długość ma zawartość pliku. Jeśli powyżej 10 znaków, dopisujemy nową zawartość, jeśli nie, pomijamy.
4. Teraz, gdy plik nieistnieje należy stworzyć dany plik. Zawartością tego pliku mają być wygenerowane pseudolosowo liczby ( maksymalna ilość tych liczb powinna być brana ze zmiennej środowiskowej - getenv). Na koniec program powinien podać ilość wygenerowanych liczb - ale tylko przez czytanie zawartości pliku!

##### Odpowiedź

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define ENV_RAND 100;
void Zadanie1do3(){
    char name[100];
    scanf("%s", name);

    FILE *file;
    file = fopen(name, "r");

    if(file != NULL){
        fclose(file);
        file = fopen(name, "a");

        char fileSize;
        fseek(file, 0, SEEK_END);
        fileSize = ftell(file);
        printf("%d",fileSize);

        if(fileSize > 10)
            fprintf(file, "\namogus");

    }else{
        file = fopen(name, "w");
        printf("No such file!");
        fprintf(file, "test");
    }

    fclose(file);

}

void Zadanie4(){
    FILE *fptr;
    char name[100];
    scanf("%s", name);

    fptr = fopen(name, "r");
    if(fptr != NULL){
        fclose(fptr);
        fptr = fopen(name, "a");
        int amount = 1;
        int max = ENV_RAND;
        char generatedMess[max];

        srand(time(NULL));
        amount = rand() % max + 1;

        for(int i = 0; i <= amount; i++){
            fprintf(fptr,"%d", rand() % amount + 1);
        }


    }else{
        fptr = fopen(name, "w");
    }



    fclose(fptr);
}

int main(){
    Zadanie3();

    return 0;
}

```

#### Zadanie 2

sys/types.h
unistd.h
execlp("ls", "ls", "-la", null)

1. Napisz program tworzący dwa procesy. Każdy ze stworzonych procesów powinien utworzyć proces - potomka. Należy wyświetlać identyfikatory procesów rodziców i potomków po każdym wywołaniu funkcji fork.
2. Napisz program którego rezultatem będzie wydruk zawartości bieżącego katalogu poprzedzony napisem „_Poczatek_" a zakończony napisem „_Koniec_"
3. Napisz program który utworzy dowolny plik o dowolnej nazwie (jeśli już taki istnieje, program powinien go otworzyć w trybie „dopisywania”). Program powinien dopisywać losowo wygenerowane ciągi znaków - liczba słów powinna być uzależniona od zmiennej środowiskowej, jak również maksymalna liczba znaków). W programie powinnny także powstać dwa osobne procesy które zrobią dokładnie to samo, co główny proces.

##### Odpowiedź 1-2:
```C
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
int main()
{
    pid_t pid1, pid2;
    printf("Process rodzica : %d\n", getpid());

    pid1 = fork();
    if(pid1 < 0){
        printf("Błąd podczas tworzenia procesu\n");
        return 1;
    }
    else if(pid1 == 0){
        printf("Pierwszy proces potomny: %d, jego rodzic: %d\n", getpid(), getppid());
    }
    else{
        printf("Proces rodzica po utowrzeniu pierwszego potomka: %d\n", getpid());
    }

    pid2 = fork();

    if(pid2 < 0){
        printf("Błąd podczas tworzenia procesu\n");
        return 1;
    }
    else if(pid2 == 0){
        printf("Drugi proces potomny: %d, jego rodzic: %d\n", getpid(), getppid());
    }
    else{
        printf("Proces rodzica po utworzeniu drugiego potomka: %d\n", getpid());
    }

    return 0;
}

```

##### Odpowiedź 3:
```C
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <time.h>
#include <stdlib.h>

#define ENV_VAR 10;

void fileCreation(char fileName[100]){
    int seed = getpid();
    srand(seed);

    FILE *fptr;
    fptr = fopen(fileName, "r");

    if(fptr != NULL){
        fclose(fptr);
        fptr = fopen(fileName, "a");
        fseek(fptr, 0, SEEK_END);

        int max = ENV_VAR;
        int openLength = ftell(fptr);
        int currChars = openLength;

        fseek(fptr, 0, SEEK_SET);
        printf("%d",currChars);

        while(currChars <= max + openLength){
            int length = rand() % max + 1;
            char word[length];

            for(int i = 0; i <= length; i++){
                word[i] = rand() % max + 1;
                fprintf(fptr, "%d", word[i]);
            }
            currChars += length;
        }
    }else{
        fptr = fopen(fileName, "w");
    }

    fclose(fptr);
}

int main()
{
    pid_t pid1, pid2;

    char fileName[100];
    scanf("%s", fileName);

    pid1 = fork();

    if(pid1 < 0){
        fprintf(stderr, "Fork failed\n");
        return 1;
    }
    else if(pid1 == 0){
        fileCreation(fileName);
    }else{
        fileCreation(fileName);
    }

    pid2 = fork();

    if(pid2 < 0){
        fprintf(stderr, "Fork failed\n");
        return 1;
    }else if(pid2 == 0){
        fileCreation(fileName);
    }else{
        fileCreation(fileName);
    }

    return 0;
}

```

#### Zadanie 3

https://michal.panczyk.info/pp/topics/9.html

##### Odpowiedź:

```C
using namespace std;  
  
void swapGreater(int *a,int *b){  
if(*a<*b){  
int tmp= *a;  
*a=*b;  
*b=tmp;  
}  
  
}  
int main(){  
int numA =25;  
int numB=30;  
  
swapGreater(&numA,&numB);  
cout<<"liczba a "<<numA<<endl;  
cout<<"liczba b "<<numB<<endl;  
return 0;  
  
}
```

```C
#include <stdio.h>

void zamienJesliWieksza(int *a, int *b) {

    if (*a > *b) {

        int temp = *a;

        *a = *b;

        *b = temp;

    }

}

int main() {

    int x = 15;

    int y = 10;

    printf("Przed zamiana: x = %d, y = %d\n", x, y);

    zamienJesliWieksza(&x, &y);

    printf("Po zamianie: x = %d, y = %d\n", x, y);

    return 0;

}
```

#### Zadanie 4

1. Napisz program który tworzy trzy procesy - proces macierzysty i jego dwa procesy potomne. Pierwszy z procesów potomnych powinien zapisać do potoku napis _„HALLO!"_, a drugi proces potomny powinien ten napis odczytać.
2. Napisz program który tworzy trzy procesy, z których dwa zapisują do potoku, a trzeci odczytuje z niego i drukuje otrzymane komunikaty.
3. Jako rozszerzenie zadania 1, proszę skorzystać z połączeń nazwanych (przy wykorzystaniu funkcji **mkfifo**
##### Odpowiedź:
###### 1.
```C
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
int main()
{
    pid_t pid1, pid2, pid3;
    int pipefd[2];

    if (pipe(pipefd) == -1){
        perror("pipe");
        return 1;
    }

    int pipeRecive = pipefd[0];
    int pipeSend = pipefd[1];

    pid1 = fork();

    if(pid1 < 0){
        printf("Process creation error!");
        return 1;
    }else if(pid1 == 0){
        //process macierzysty
        pid2 = fork();

        if(pid2 < 0){
            printf("Process creation error!");
            return 1;
        }else if(pid2 == 0){
            //proces potomny 1
            char msgWrite[] = "HALLO!";
            char msgLength = sizeof(msgWrite);

            close(pipeRecive);
            write(pipeSend, msgWrite, msgLength);
            printf("Wysyłam\n");
            close(pipeSend);
            wait(NULL);

        }else{
            pid3 = fork();

            if(pid3 < 0){
                printf("Process creation error!");
                return 1;
            }else if(pid3 == 0){
                //proces potomny 3

                char msgRead[20];

                printf("czytam\n");
                read(pipeRecive, msgRead, sizeof(msgRead));
                close(pipeRecive);

                printf("%s", msgRead);
                wait(NULL);
            }else{
                //proces jakis tam
            }
        }
    }else{
        close(pipeSend);
        close(pipeRecive);
    }

    return 0;
}

```



###### 2.
```C
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>


int main()
{
    int pipefd[2];
    int pipefd2[2];
    pid_t pid1, pid2, pid3;
    char output[12];

    if (pipe(pipefd) == -1) {
        perror("pipe");
        return 1;
    }
    if (pipe(pipefd2) == -1) {
        perror("pipe");
        return 1;
    }

    pid1 = fork();

    if(pid1 < 0){

    }else if(pid1 == 0){
        //proces 1
        char message1[] = "He he";

        close(pipefd[0]);

        write(pipefd[1], message1, sizeof(message1) + 1);
        close(pipefd[1]);

    }else{
        pid2 = fork();

        if(pid2 < 0){

        }else if (pid2 == 0){
            //proces 2
            char message2[] = "Ha Ha";

            close(pipefd2[0]);

            write(pipefd2[1], message2, sizeof(message2) + 1);
            close(pipefd2[1]);
        }else{
            pid3 = fork();

            if(pid3 < 0){

            }else if (pid3 == 0){

            } else{
                // program główny
                char newOutput[120];

                close(pipefd[1]);
                read(pipefd[0], output, 12);

                int firstSize = sizeof(output);

                for(int i = 0; i < sizeof(output); i++){
                    newOutput[i] = output[i];
                }

                close(pipefd2[1]);
                read(pipefd2[0], output, 12);

                for(int i = 0; i < sizeof(output); i++){
                    newOutput[i + 5] = output[i];
                }

                close(pipefd[0]);
                close(pipefd2[0]);

                printf("Wiadomość odczytana: %s", newOutput);
            }
        }

    }

    return 0;
}

```

#### Zadanie 5

1. Napisz program który tworzy wątek (pthread_create) który między innymi przyjmie parametr funkcji robiącej dowolne operację np. wyświetlenie zawartości 10 elementowej tablicy. Następnie należy zamknąć wcześniej uruchomiony wątek (pthread_join, pthread_cancel).

2. Do poprzedniego programu założyć zmienną globalną. Ta zmienna globalna ma być ikrementowana 10 razy jednocześnie przez wątek( w funkcji przekazywanej do tworzenia wątku) i proces główny.

3. Skorzystać z mutexów (pthread_mutex_t) w celu zablokowania i zmiany asynchronicznej wartości globalnej zmiennej (pthread_mutex_lock(), pthread_mutex_unlock())

#### Zadanie 6

1. Napisz program który tworzy wątek (pthread_create) który między innymi przyjmie parametr funkcji robiącej dowolne operację np. wyświetlenie zawartości 10 elementowej tablicy. Następnie należy zamknąć wcześniej uruchomiony wątek (pthread_join, pthread_cancel).

2. 2. Do poprzedniego programu założyć zmienną globalną. Ta zmienna globalna ma być ikrementowana 10 razy jednocześnie przez wątek( w funkcji przekazywanej do tworzenia wątku) i proces główny.

3. Skorzystać z mutexów (pthread_mutex_t) w celu zablokowania i zmiany asynchronicznej wartości globalnej zmiennej (pthread_mutex_lock(), pthread_mutex_unlock())

#### Zadanie 7

https://www.geeksforgeeks.org/ipc-using-message-queues/

#### Zadanie 8

Obsługa za pomocą metod: shmget, shmctl, shmat.

Napisać dwa programy komunikujące się przez pamięć współdzieloną. Pierwszy w nieskończonej pętli, naprzemiennie wysyła napisy „CIEPLO”, „ZIMNO” do pamięci współdzielonej. Drugi odczytuje z pamięci wartości (proszę pamiętać o walidacji czy dany napis jest poprawny - czy „CIEPLO, czy „ZIMNO” - jeśli niepoprawny, powinien zostać wyświetlony błąd).

#### Zadanie 9

Dodać funkcjonalność "zamykania" i "otwierania" semafora w obydwu programach z poprzednich zajęć. Powinny one otwierać dostęp do zmiennej która jest przekazywana w pamięci współdzielonej - zalecam przekształcenie jej na strukturę której atrybut będzie stanowił ciąg znaków.

[http://www.vishalchovatiya.com/semaphore-between-processes-example-in-c/](http://www.vishalchovatiya.com/semaphore-between-processes-example-in-c/ "http://www.vishalchovatiya.com/semaphore-between-processes-example-in-c/")

[https://eric-lo.gitbook.io/synchronization/semaphore-in-c](https://eric-lo.gitbook.io/synchronization/semaphore-in-c "https://eric-lo.gitbook.io/synchronization/semaphore-in-c")

[https://riptutorial.com/c/example/31715/semaphores](https://riptutorial.com/c/example/31715/semaphores "https://riptutorial.com/c/example/31715/semaphores") - metoda IPC ("od zera")