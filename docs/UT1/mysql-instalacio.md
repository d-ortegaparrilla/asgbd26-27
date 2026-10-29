# Instal·lació de MySQL
Els paquets de distribució per a MySQL estan disponibles per a diverses plataformes, incloses les distribucions de Linux, Windows i macOS. Aquests paquets proporcionen una manera flexible i ràpida de començar a utilitzar MySQL. L'última versió LTS de MySQL és la 8.4.

## Instal·lació en Linux
En Linux tenim diverses possibilitats per a fer la instal·lació:

- Descarregar el paquet amb la versió de MySQL per al nostre sistema operatiu de [https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/). 
- Des de l'anterior pàgina es poden configurar els repositoris oficial de MySQL compatibles con distintes distribucions Linux.
- Utilitzar els repositoris de la distribució per a fer la instal·lació. Moltes distribucions opten per incorporar el fork MariaDB Server front MySQL Community Edition.
- Compilar la nostra pròpia versió des del codi font.

### Instal·lació des del repositori oficial APT
A distribucions basades en Debian/Ubuntu configurarem el repositori oficial APT. Primerament s'ha d'obtindre l'arxiu **deb** que permet la configuració del repositori oficial. El nom de l'arxiu té el format `mysql-apt-config_{version-number}_all.deb`

{version-number}: Versió de la configuració del repositori de MySQL.

Instal·la el deb descarregat al SO:

    $ sudo dpkg -i mysql-apt-config_{version-number}_all.deb

Instal·la MySQL amb el següent comandament:

    $ sudo apt install mysql-server

Açò instal·la el paquet de MySQL server (mysql-server) i els paquets per al components requerits per al servidor, incloent els paquets del client (mysql-community-client), els missatges comuns d'errors i el conjunt de caràcters per al client i el servidor (mysql-community-common), i les libreries compartides (mysql-community-libs).

En el procés d'instal·lació del paquet es demana una contrasenya per al superusuari `'root'@'localhost'`.

A Ubuntu el servidor s'arranca automàticament després de l'instalació però és interessant conèixer els comandaments següents.

Per arrancar el servidor de MySQL:

    $ sudo systemctl start mysql

Per comprovar l'estat del servidor:

    $ systemctl status mysql

Per activar el servidor i que arranque a l'iniciar de nou el SO:

    $ sudo systemctl enable mysql

Es convenient consultar el fitxer de registre d'errors `/var/log/mysql/error.log` per comprovar que el servidor ha arrancat correctament.

## Primers passos amb MySQL

Per comprovar el funcionament del servidor accedim a ell amb el client:

    $ mysql -u root -p

Podem passar un script SQL perquè s'execute en el moment d'accedir amb el client:

    $ mysql -u root -p < "arxiu.sql"

Algunes ordres bàsiques són:

    SHOW databases; -- mostra les BD existents al servidor
    USE nom_bbdd; -- accedeix a una BD
    SHOW tables; -- mostra les taules d'una BD
    DESCRIBE nom_taula; -- mostra els detalls d'una taula (columnes, tipus, claus...)
    CREATE DATABASE nom_bbdd; -- crea una BD
    DROP DATABASE nom_bbdd; -- esborra una BD


## Exercici
Criteris d'avaluació 1f, 1g, 1h, 1i, 1j.

**Instal·lació de MySQL Community Edition**

- Aquesta pràctica es realitzarà dins el Learner Lab d'AWS del mòdul. Documenta amb captures de pantalla els passos seguits.
- Crea una instància d'EC2 **t3.small**.
- El SO de la instància EC2 serà una distribució Linux Ubuntu.
- Crea un **security group** anomenat **SSH** que permeta la connexió SSH des de qualsevol direcció IP (mala pràctica). Assigna aquest **security group** a la nova instància.
- La configuració de la instància es realitzarà via SSH des de l'equip client de l'alumne. Genera una clau privada **.pem** en AWS per fer les connexions SSH i assigna-les a la instància. 
- Realitza la instal·lació seguint els passos necessaris. Comprova que està en marxa el servidor i fes que el servei es pose en marxa automàticament després de l'inici de la instància. 
- Una vegada realitzada la instal·lació mostra el contingut de l'arxiu de log d'errors i connecta't amb el client de MySQL amb l'usuari `'root@localhost'`. Si tot funciona correctament, ix del client.
-  Anem a crear una BD d'exemple i a inserir dades a ella. Per això, descarrega els arxius **.zip** de les BD d'exemple **sakila** i **world** de la pàgina [https://dev.mysql.com/doc/index-other.html](https://dev.mysql.com/doc/index-other.html) i executa els scripts per crear auquestes BD. Executa alguna de les ordres bàsiques per mostrar que s'han creat correctament les dades (SHOW DATABASES, SHOW TABLES, SELECTS...).
- Després de finalitzar la pràctica atura la instància EC2.




