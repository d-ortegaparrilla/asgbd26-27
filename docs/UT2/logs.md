# Fitxers de logs 
El servidor de MySQL té diversos arxius de logs que poden ajudar-nos a conèixer quina activitat està tenint lloc al servidor:

- **Log d'errors**. Problemes trobats iniciant, executant o parant mysqld.
- **Undo logs**. Sentències que canvien dades dins una transacció.
- **Redo logs**. Sentències que canvien dades de transaccions que no s'han escrit en disc. Vist a l'anterior apartat.
- **Log binari**. Sentències que canvien dades. Donen suport per a recuperació.
- **Log de consultes lentes**. Sentències que van tardar més de **long_query_time** segons, o no van utilitzar índexs.
- **Log general de consultes**. Connexions de client establides i sentències executades.


## Log d'errors
S'activa per defecte. El log d'errors conté un registre dels temps d'inici i tancament de mysqld. També conté missatges de diagnòstic com ara errors, advertències i notes que es produeixen durant l'inici i l'apagat del servidor i mentre el servidor s'està executant. Per exemple, si mysqld observa que una taula s'ha de comprovar o reparar automàticament, escriu un missatge al registre d'errors. La variable **log_error** indica la ruta a l'arxiu.

## Undo log
Els undo logs garanteixen l'atomicitat (desfent transaccions no confirmades) i l'aïllament (admetent lectures consistents dins una transacció). Els registres es guarden per defecte al directori **datadir**.

## Redo log
Els redo log formen part del protocol **Write-Ahead Logging** (**WAL**) de MySQL InnoDB, que garanteixen la durabilitat registrant els canvis abans que s'escriguen als fitxers de dades. Això garanteix que les transaccions confirmades es puguin recuperar després d'un error. Els registres es guarden per defecte al directori **datadir**.

Per a més informació consultar l'apartat anterior de [grandària del registre de transaccions](innodb.md)

## Log binari
En ell es guarden totes les sentències que han actualitzat dades o podrien haver-lo fet (per exemple, un UPDATE que finalment no va actualitzar cap registre). Els registres es guarden per defecte al directori **datadir**.

És necessari per la replicació i també es pot utilitzar per a restaurar dades després d'una còpia de seguretat a un punt concret, ja que conté totes les actualitzacions que s'han fet. Aquest tipus de restauració es coneix com a point-in-time recovery (**PITR**).

En les últimes versions de MySQL es troba actiu per defecte (no a les antigues)

Podem utilitzar les variables **log_bin** per activar-lo (ON u OFF). Els registres es guarden per defecte al directori **datadir**.

Amb **SHOW** podem comprovar els noms del arxius log binaris:

    SHOW BINARY LOGS;

Podem consultar el contingut d'aquest arxiu de la següent manera:

    $> mysqlbinlog log_file

Amb el paràmetre **-d** podem filtrar el contingut del registre a una BD específica.

    $> mysqlbinlog log_file -d database


## Log de consultes lentes
El registre de consultes lent es pot utilitzar per trobar consultes que tarden molt de temps a executar-se i, per tant, són candidates a realitzar un procés d'optimització. Tota consulta que supere el temps determinat a la variable **long_query_time** es considera una consulta lenta. Podem determinar que les consultes que no es facen fent ús d'índexs també siguen considerades com a lentes amb la variable **long_queries_not_using_indexes**.

Per defecte està desactivat, havent d'utilitzar la variable **slow_query_log** per activar-lo. L'arxiu per defecte és *host_name-slow.log*. Es pot configurar aquest arxiu amb la variable **slow_query_log_file**.

## Log general de consultes
És de gran utilitat quan sospitem que s'estan produint errors en un client i es vol conèixer què va enviar exactament el client al servidor.

Per activar-lo hem de configurar la variable **general_log** amb valor **'ON'**. Per defecte el registre es guarda a l'arxiu *host_name.log*. Es pot canviar el nom de l'arxiu amb la variable **general_log_file**.


## Exercici
Criteris d'avaluació 2f, 2h.

- Activa tots els logs vists als apunts.
- Comprova que les variables s'han configurat correctament amb el comandament **SHOW**.
- Els següents passos es realitzaran a la BD **sakila**.
- Realitza un parell de consultes de prova. Una d'elles forçarem que siga lenta amb la instrucció **SELECT sleep(*num_segons*)**.
- Realitza una instrucció que inserisca un registre a una taula de la BD (INSERT).
- Mostra el contingut dels logs de consultes, consultes lentes i binari, per a les accions realitzades a la BD **sakila**.