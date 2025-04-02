# Komunikacja sieciowa w Pythonie

## Wprowadzenie

Moduł `socket` w Pythonie udostępnia interfejs do Berkeley sockets API i jest wykorzystywany do komunikacji sieciowej. Podstawowe funkcje i metody w tym module to:

- `socket()`, `bind()`, `listen()`, `accept()`
- `connect()`, `connect_ex()`, `send()`, `recv()`, `close()`

Python zapewnia spójne API, które odwzorowuje systemowe wywołania w C. Istnieją także klasy ułatwiające korzystanie z tych funkcji, np. moduł `socketserver`, który upraszcza tworzenie serwerów sieciowych. Python obsługuje również wyższe protokoły, takie jak HTTP i SMTP.

### Dlaczego TCP?

Protokół TCP:
- **Jest niezawodny** – wykrywa i retransmituje zgubione pakiety.
- **Gwarantuje kolejność danych** – odbiorca otrzymuje dane w takiej samej kolejności, w jakiej zostały wysłane.

W przeciwieństwie do tego, gniazda UDP (`socket.SOCK_DGRAM`) nie zapewniają niezawodności ani kolejności danych, ale oferują większą wydajność dzięki mniejszym narzutom czasowym.

### Dlaczego to ważne?

Sieci nie gwarantują dostarczenia danych – mogą wystąpić opóźnienia, zgubione pakiety czy ograniczenia sprzętowe. TCP automatycznie zarządza tymi problemami, zapewniając stabilną komunikację.
Schemat typowej komunikacji klient – serwer przedstawiono na rysunku poniżej:

![image](https://github.com/user-attachments/assets/fd1e54e2-56bd-4a1c-a7aa-298c3a56ad69)

Lewa kolumna reprezentuje serwer, a prawa klienta.
W górnej części po lewej stronie znajdują się wywołania API, które serwer wykonuje, aby utworzyć gniazdo „nasłuchujące”:
• `socket()`
• `.bind()`
• `.listen()`
• `.accept()`
Gniazdo nasłuchujące działa zgodnie z nazwą – oczekuje na połączenia od klientów. Gdy klient się połączy, serwer wywołuje `.accept()`, aby zaakceptować połączenie.
Klient wywołuje `.connect()`, aby nawiązać połączenie z serwerem i rozpocząć tzw. _handshake_ (trójetapowe uzgadnianie połączenia). Ten proces zapewnia, że każda strona połączenia jest osiągalna w sieci.
W środkowej części następuje wymiana danych między klientem a serwerem przy użyciu `.send()` i `.recv()`.
Na końcu zarówno klient, jak i serwer zamykają swoje gniazda.

_**Wskazówka:**_ Więcej przykładów i dokumentacji znajdziecie w oficjalnej dokumentacji Pythona: Python Socket Documentation.

## Przykład podstawowej komunikacji klient-serwer

Poniżej znajduje się przykład prostego serwera i klienta TCP:

_**Zadanie:**_ Uruchom przykład z: https://docs.python.org/3/library/socket.html#example i zaobserwuj działanie. Zmień port, na którym komunikują się programy. Czy każdy dowolny port jest
dostępny do komunikacji?

## Zadania do wykonania 

#### 1. Prosty klient-serwer (powitanie)
Stwórz aplikację składającą się z dwóch programów: serwera i klienta.

Serwer:
- Czeka na jedno połączenie.
- Po nawiązaniu połączenia czeka na wiadomość od klienta.
- Jeśli klient wyśle wiadomość _"Hello"_, serwer odpowiada _"Hello from server"_ i kończy połączenie.
- Jeśli klient wyśle wiadomość _"Time"_, serwer odpowiada, wysyłając klientowi aktualny czas.
- Jeśli to inna wiadomość, serwer odpowiada _"Unknown command"_.

Klient:
- Łączy się z serwerem.
- Wysyła wiadomość _"Hello"_.
- Odbiera odpowiedź z serwera i wyświetla ją na ekranie.

#### 2: Gra „Papier, Nożyce, Kamień” (PvP do 3 wygranych)
Zaimplementuj serwer TCP umożliwiający rozgrywkę pomiędzy dwoma użytkownikami.
- Serwer czeka na połączenie dokładnie dwóch graczy.
- Każdy gracz wybiera jedną z opcji: _"papier"_, _"nożyce"_ lub _"kamień"_ i wysyła ją na serwer.
- Po otrzymaniu wyboru od obu graczy serwer:
- Oblicza wynik rundy.
- Wysyła do obu graczy informację o wyborze przeciwnika oraz wynik rundy.
- Rozgrywka trwa do momentu, gdy jeden z graczy osiągnie 3 zwycięstwa.
- Po zakończeniu gry serwer informuje graczy o wyniku końcowym i zamyka połączenia.

#### 3: Chat wieloosobowy z administracją
Stwórz prostą aplikację czatu TCP umożliwiającą komunikację wielu użytkowników jednocześnie:
- Każdy klient po podłączeniu musi podać swój nick.
- Każda wiadomość wysłana przez klienta jest rozsyłana do wszystkich podłączonych użytkowników.
- Osoba korzystająca z aplikacji serwera występuje jako _admin_ i także może pisać wiadomości.
- Admin posiada dodatkowo specjalną komendę _/kick <nick>_, umożliwiającą wyrzucenie wybranego użytkownika z czatu.
- Każde zdarzenie (dołączenie, opuszczenie czatu, wyrzucenie użytkownika) powinno być komunikowane wszystkim uczestnikom.

#### 4: Prosty skaner portów
Napisz program, który skanuje określony zakres portów na danym hoście i sprawdza, które z nich są otwarte.

#### 5: Serwer czasu
Zaimplementuj serwer, który zwraca aktualny czas w formacie UTC po otrzymaniu komendy _"TIME"_.
