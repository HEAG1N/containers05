IWNO6: Interacțiunea containerelor

Scopul lucrării

După finalizarea acestei lucrări studentul va fi capabil să gestioneze interacțiunea între mai multe containere.
Sarcina

Creați o aplicație PHP pe baza a două containere: nginx, php-fpm.
Pregătire

Pentru efectuarea acestei lucrări este necesar să avem instalat pe computer Docker.

Pentru efectuarea lucrării este necesar să avem experiență în efectuarea lucrării de laborator №3.
Efectuarea lucrării

Cream un repozitoriu containers05 și il copiam pe computerul dvs.


În directorul containers05 cream directorul mounts/site. În acest director copiam site-ul PHP creat în cadrul cursului Programare PHP.

Cream fișierul .gitignore în rădăcina proiectului și adăugam următoarele linii:

![428401371-a73368f2-ef3a-4b92-8dd1-5cc5cd2f9ba5](https://github.com/user-attachments/assets/ebf3a1ce-b573-4891-9ae9-3c0db2414241)

Cream în directorul containers05 fișierul nginx/default.conf cu următorul conținut:

![428401379-3ed6f07b-91a6-48d7-808a-d893066475cc](https://github.com/user-attachments/assets/2e4285f2-d51c-4da1-9357-e7a975d8df39)

Pornirea și testarea

Cream rețeaua internal pentru containere.

![428401389-fb70e55a-4280-4694-b3f6-ffc9d6ce4465](https://github.com/user-attachments/assets/c1f667b9-2622-4a0e-9ae1-24836e44a070)

Cream containerul backend cu următoarele proprietăți:

pe baza imaginii php:7.4-fpm;

directorul mounts/site este montat în /var/www/html;

funcționează în rețeaua internal.

![428401399-97a070e6-d4ed-4687-bccd-13c2138f7374](https://github.com/user-attachments/assets/3fd7a54f-120b-4a8f-aea4-8162ffac8e49)

Cream containerul frontend cu următoarele proprietăți:

pe baza imaginii nginx:1.23-alpine;

directorul mounts/site este montat în /var/www/html;

fișierul nginx/default.conf este montat în /etc/nginx/conf.d/default.conf;

portul 80 al containerului este expus pe portul 80 al calculatorului gazdei;

funcționează în rețeaua internal.

![428401411-76596553-7f8d-4d6f-88a2-f1efc0b8594e](https://github.com/user-attachments/assets/fde7cbbc-e16f-4dba-a4c9-426d87fafb88)

Pentru a testa funcționarea site-ului, deschidem site-ul în browser, trecând la adresa http://localhost.

![428401420-f2a042ca-c8cf-4dee-8621-ae7e3b3b94aa](https://github.com/user-attachments/assets/43ae0f2c-5a34-4c22-a850-9ddbb7f3cbd2)

Răspunsuri la întrebări:

În ce mod în acest exemplu containerele pot interacționa unul cu celălalt?

Containerele interacționează prin rețeaua Docker numită internal. Nginx (frontend) comunică cu PHP-FPM (backend) prin această rețea, trimițând cereri pentru procesarea fișierelor PHP. Comunicarea este configurată în fișierul default.conf al Nginx prin directiva fastcgi_pass backend:9000, unde "backend" este numele containerului PHP-FPM în rețeaua respectivă.

Cum văd containerele unul pe celălalt în cadrul rețelei internal?

În cadrul rețelei Docker internal, containerele își pot vedea reciproc folosind numele containerelor ca nume de host. De exemplu, containerul Nginx poate accesa containerul PHP-FPM folosind numele "backend" și portul 9000, iar PHP-FPM poate accesa containerul Nginx folosind numele "frontend". Docker DNS resolve automat aceste nume la adresele IP corespunzătoare din rețeaua internă, facilitând comunicarea între containere fără a fi necesară cunoașterea adreselor IP specifice.

De ce a fost necesar să se suprascrie configurarea nginx?

A fost necesar să se suprascrie configurarea implicită Nginx pentru a:

Configura Nginx să transmită cererile pentru fișierele PHP către containerul PHP-FPM (backend) prin fastcgi_pass

Defini calea corectă către documentele web în /var/www/html

Configura routingul pentru a permite framework-uri PHP moderne (prin directiva try_files)

Seta parametrii FastCGI necesari pentru ca PHP-FPM să poată procesa corect cererile

Configurarea implicită a Nginx nu include aceste setări specifice pentru PHP și nu știe despre existența unui container PHP-FPM separat care să proceseze fișierele PHP.

Concluzii

Prin această lucrare de laborator am învățat cum să configurez și să gestionez interacțiunea între mai multe containere Docker. Am reușit să creez un mediu de dezvoltare pentru o aplicație PHP folosind containerele Nginx și PHP-FPM, care comunică între ele printr-o rețea Docker dedicată. Această abordare de separare a responsabilităților între containere oferă mai multă flexibilitate și modularitate, permițând actualizarea sau înlocuirea unui container fără a afecta celelalte componente ale aplicației. De asemenea, am învățat cum să configurez corect volumele pentru a partaja date între containere și host, precum și cum să configurez Nginx pentru a lucra cu PHP-FPM într-un mediu containerizat.
