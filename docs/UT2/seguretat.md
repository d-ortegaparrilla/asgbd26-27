# Seguretat en MySQL
## Restauració de la contrasenya del usuari root
Primerament haurem d'activar la variable de sistema `skip-grant-tables` al servidor de mysqld.

    [mysqld]
    ...
    skip-grant-tables = ON
    ...

Reiniciem el servidor:

    sudo systemctl restart mysql.service

D'aquesta manera el servidor de mysql no controla els passwords dels usuaris i podem accedir a ell directament amb:

    mysql

Una vegada dins la consola de mysql carreguem les taules de permisos perquè puguem canviar la contrasenya.

```sql
mysql> FLUSH PRIVILEGES;
```

La següent instrucció canviaria la contrasenya de l'usuari root:

```sql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Nova contrasenya';
```

Només quedaria eixir de la consola de mysql, deixarem l'arxiu de configuració al seu estat inicial i reiniciem el servei de mysql.

## Configuració de connexions encriptades
Es pot configurar MySQL perquè les connexions al servidor es realitzen encriptades.

Per demanar que els clients es connecten mitjançant connexions xifrades, s'ha d'activar la variable de sistema **require_secure_transport**. 

Les següents variables de sistema del costat del servidor especifiquen els fitxers de certificat i clau que fa servir el servidor quan permet als clients establir connexions xifrades:

- **ssl_ca**: nom del fitxer de certificat de l'autoritat de certificació (CA). 
- **ssl_cert**: nom del fitxer de certificat de clau pública del servidor. Aquest certificat es pot enviar al client i autenticar-se amb el certificat CA que té.
- **ssl_key**: nom del fitxer de clau privada del servidor.

Aquests arxius es busquen al directori de dades de MySQL, definit a la variable de sistema **datadir**. El servidor MySQL és capaç de crear a l'inici els arxius dels certificats que falten.

Per exemple, per habilitar el servidor per a connexions xifrades, es pot afegir les següents línies al fitxer **my.cnf**, canviant els noms dels fitxers segons siga necessari:

    [mysqld]
    ssl_ca=ca.pem
    ssl_cert=server-cert.pem
    ssl_key=server-key.pem

**Nota**: hi ha instal·lacions de MySQL que per defecte donen valor a les variables **ssl_ca**, **ssl_cert** i **ssl_key** (al contrari del que es diu a la documentació). Els arxius corresponents fan referència a un certificat autosignat. Es pot configurar MySQL perquè utilitze un certificat signat per una entitat certificadora reconeguda.

Si volem obligar que els clients realitzen les connexions xifrades afegeríem las següent línia:

    [mysqld]
    require_secure_transport=ON

## Configuració de les connexions xifrades al client
Podem establir com volem que es facen les connexions des del client cap al servidor. Podem fer-ho amb l'opció **ssl-mode**, bé des de la línea de comandament, bé des de l'arxiu de configuració del client:

    [mysql]
    ssl-mode = MODE_CONNEXIO

Els modes de connexions són:

- DISABLED: sense xifrar.
- PREFERRED: xifra les connexions si el servidor les suporta. És l'opció per defecte.
- REQUIRED: xifra obligatòriament les connexions. Si el servidor no les suporta aborta la connexió.
- VERIFY_CA: com REQUIRED però a més exigeix que hi haja un certificat CA (Certificate Authority) vàlid per al servidor.
- VERIFY_IDENTITY: com VERIFY_CA però a més exigeix que el hostname del servidor coincidisca amb el CN (o SAN) del CA del servidor.

Per comprovar si la connexió del client amb el servidor està xifrada podem consultar la variable d'estat **Ssl_version**. Si no té cap valor és que la connexió no està xifrada.


## Accés remot al servidor
Per accedir de manera remota al servidor crearem un usuari que puga connectar-se des de qualsevol direcció. Limitarem els permisos d'aquest usuari a una BD específica.

    CREATE USER nom_usuari IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON nom_bd.* TO nom_usuari;


## Exercici
Criteris d'avaluació 2c, 2d, 2e, 2h.

- Configura el servidor MySQL perquè les connexions es realitzen via SSL. Crea un usuari que puga connectar-se remotament i dóna-li tots els permisos a la BD que es va crear a la UT1.

- Configura la connexió SSL a la BD des d'un equip client amb el programa DataGrip i amb el client en mode text mysql.
  
- Comprova distintes opcions per establir les connexions xifrades tant al client com al servidor i comprova que els resultats són els esperats.
