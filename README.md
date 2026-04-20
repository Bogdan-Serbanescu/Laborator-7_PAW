-1. De ce Logout este implementat ca <form method="post"> și nu ca un link <a href="/Auth/Logout">?
Ce s-ar putea întâmpla dacă logout-ul ar fi un GET accesibil printr-un link?

Un GET ar permite atacuri CSRF: orice pagină externă poate plasa <img src="/Auth/Logout">, iar un utilizator autentificat care o vizitează ar fi deconectat involuntar. Formularul POST include un token anti-forgery generat automat de ASP.NET Core, care blochează astfel de atacuri

-2. De ce login-ul face doi pași în loc de unul?

PasswordSignInAsync caută după UserName, nu după Email. Dacă am trimite email-ul direct, Identity l-ar interpreta ca UserName și autentificarea ar eșua.

-3

Ascunderea in View nu protejeaza impotriva unui request, doar ascunde optiunile vizuale. Invers, duce la confuszii cu utilizatori care vad butoanele dar cand le apasa ii directioneaza la login sau le da eroare.

-4

-Un lanț de componente prin care trece fiecare request HTTP în ordine. Fiecare componentă poate procesa sau modifica request-ul.
-Dacă le inversăm, UseAuthorization rulează cu un user neidentificat și toate rutele [Authorize] returnează 401, indiferent dacă utilizatorul are un cookie valid.

-5

Ce am fi trebuit să implementăm manual fără Identity?

Tot ce ofera Identity:

Hashing parole — PBKDF2/bcrypt; stocarea în clar ar fi o vulnerabilitate critică
Înregistrare — validare unicitate email/username, politici de parolă
Autentificare — verificarea hash-ului
Sesiuni și cookie-uri — generare token securizat, emitere și validare cookie
Protecție CSRF — token-uri anti-forgery pentru formulare
Lockout — blocare cont după încercări eșuate repetate
Roluri și claims — sistem de permisiuni
Resetare parolă — token-uri cu expirare, trimitere email


-6

Rezolvare: disavantaje Identity:
Cuplare tehnologică — Identity presupune Entity Framework Core și o bază de date relațională. Integrarea cu alte store-uri non-relaționale este mai complexa.
Schema rigida — tabelele au o structură fixă. Modificarea la un alt sistem  necesită transformări manuale și resetarea parolelor.
Nepotrivit nativ pentru API-uri — Identity este construit pentru autentificare cu cookie-uri. Dacă expui un API REST pentru mobile sau Angular/React, trebuie să adaugat separat JWT, complicând arhitectura.
