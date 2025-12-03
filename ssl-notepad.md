Wymagania wstępne (Kluczowe!)
Zanim klikniesz "Save" w panelu, upewnij się, że:

DNS działa: Domena webgis.giswgorach.pl wskazuje na publiczne IP twojego serwera.

Port 80 jest otwarty: Firewall serwera (np. ufw, iptables) oraz ewentualnie router/grupa zabezpieczeń u dostawcy VPS muszą przepuszczać ruch na porcie 80. Let's Encrypt używa portu 80 do weryfikacji (tzw. ACME Challenge), nawet jeśli potem ruch idzie przez 443.

2. Konfiguracja w panelu Nginx Proxy Manager
Zaloguj się do NPM (zazwyczaj http://twoje-ip:81).

Przejdź do Hosts -> Proxy Hosts.

Kliknij przycisk Add Proxy Host w prawym górnym rogu.

Zakładka "Details":
Domain Names: wpisz webgis.giswgorach.pl i wciśnij Enter.

Scheme: http (zostaw domyślne, NPM komunikuje się z Lizmapem wewnątrz po HTTP).

Forward Hostname / IP: wpisz web (to nazwa usługi kontenera nginx z docker-compose).

Forward Port: 80 (wewnętrzny port kontenera web).

Cache Assets / Block Common Exploits: Zalecam zaznaczyć obie opcje.

Zakładka "SSL" (Tu dzieje się magia):
SSL Certificate: Wybierz z listy: "Request a new SSL Certificate".

Force SSL: [Zaznacz] (Przekieruje automatycznie HTTP na HTTPS).

HTTP/2 Support: [Zaznacz] (Poprawia wydajność ładowania map).

Email Address for Let's Encrypt: Podaj swój e-mail (do powiadomień o wygasaniu, jeśli automat zawiedzie).

I Agree to the Let's Encrypt Terms of Service: [Zaznacz].

Kliknij Save.

Chwilę to potrwa (od 5 do 30 sekund). Jeśli wszystko jest OK, okienko się zamknie, a na liście pojawi się Twoja domena ze statusem "Online" i tarczą SSL.

3. Dostosowanie Lizmapa (Fix dla Mixed Content)
Jako że ruch szyfrowany kończy się na NPM, a do Lizmapa trafia jako zwykłe HTTP, Lizmap może próbować ładować style CSS i skrypty po HTTP, co przeglądarka zablokuje (strona będzie "rozsypana").

Musisz wymusić HTTPS w konfiguracji Lizmapa (jeśli jeszcze tego nie zrobiłeś):

Edytuj plik lizmap/var/config/localconfig.ini.php (ścieżka zależy od tego, gdzie mapujesz wolumeny na dysku).

Upewnij się, że sekcja [main] zawiera wpis:

Ini, TOML

[main]
isHttps = 1
