# Configuració d'InnoDB
El motor d'InnoDB és complexe i no podem aspirar a tractar en profunditat el seu funcionament i personalització. A continuació es mostren alguns paràmetres de configuració importants del motor.

## Grandària de la memòria cau (innodb_buffer_pool_size)
És la memòria principal d'innodb (**buffer pool**) i permet estalviar accessos a disc.

**Exemple**: BD de 500GB, amb les dades dels últims 10 anys, però només es consulten sovint els dels últims 2 mesos. Amb un buffer de (500/120*2) apróx. 10 Gb seria suficient.

Quan el servidor té activada la variable **innodb_dedicated_server** proporciona el següents valors al buffer:

| Memòria detectada al servidor | Grandària del buffer pool 
| ----------------------------- | -------------------------
| Menor de 1 GB | 128MB (valor per defecte)
| Entre 1GB i 4GB | memòria detectada * 0,5
| Més de 4GB | memòria detectada * 0,75

[grandaria]## Grandària del registre de transaccions (innodb_redo_log_capacity)
El registre de transaccions (**redo log**) és una estructura utilitzada quan s'ha de realitzar una recuperació de les dades quan s'ha executat alguna escriptura en una transacció incompleta. L'objectiu principal és garantir la durabilitat de les dades proporcionant un sistema per a desfer les transacciones incompletes. Als fitxers redo logs s'escriuen totes les escriptures de dades realitzades abans del commit d'una transacció.

Determinar la grandària correcta d'aquests fitxers és fonamental perquè MySQL funcione correctament. El següent és un exemple d'error que podem trobar-nos si tenim una grandària insuficient dels fitxers de redo logs.

    [ERROR] InnoDB: The total blob data length (12299456) is greater than 10% of the total redo log size (100663296). Please increase total redo log size.

La variable **innodb_redo_log_capacity** permet establir la grandària dels fitxers, sent el seu valor per defecte insuficient a sistemes amb volum d'escriptura elevat.

Quan el servidor té activada la variable **innodb_dedicated_server** la grandària del registre es calcula d'acord al nombre de processadors lògics disponibles al servidor. La fórmula és (número de processadors lògics / 2) GB, amb un valor màxim de 16GB.



## Transaccions
A InnoDB es realitza per defecte un autocommit de cada instrucció. Per tant, cada instrucció s'executaria en una transacció distinta. Podem modificar aquest comportament amb la variable de sistema **autocommit**. Desactivar l'autocommit pot millorar el rendiment del servidor, ja que allibera de la creació i terminació continu de les transaccions.

Podem utilitzar la instrucció **START TRANSACTION** per forçar l'execució de més d'una instrucció dins la mateixa transacció.

```sql
START TRANSACTION;
UPDATE Compte SET saldo = saldo - 100 WHERE id_compte = 1;
UPDATE Compte SET saldo = saldo + 100 WHERE id_compte = 2;
COMMIT;
```

Podem revertir les instruccions realitzades a una transacció si ocorreix un error.

```sql
START TRANSACTION;
UPDATE Inventari SET stock = stock - 10 WHERE id_producte = 5;
IF (SELECT stock FROM Inventari WHERE id_producte = 5) < 0 THEN
    ROLLBACK;
ELSE
    COMMIT;
END IF;
```


## Aïllament de dades (transaction_isolation)
Ajustar aquest paràmetres pot tenir gran impacte sobre el rendiment, però també sobre la seguretat de les dades. Els possibles valors són: 

- **REPEATABLE_READ**. És el valor per defecte a InnoDB. Assegura lectures consistents dins la mateixa transacció. Totes les consultes veuren el mateix snapshot de les dades, establit per la primera lectura.
- **READ_COMMITED**. És el valor per defecte a molts SGBD, com PostgreSQL, Oracle and SQL Server. Cada lectura, inclòs dins d'una mateixa transacció, crea i llig un nou snapshot de les dades. Açò pot provocar **phantom reads** si una altra transacció realitza un commit on afegeix o elimina dades en meitat de l'execució de la 1a transacció. Com avantatge és que no hi ha bloquejos, permeten la lliure inserció de nous registres.
- **READ_UNCOMMITED**. Amb aquest valor la 1a transacció pot llegir els canvis que realitza una altra transacció abans que aquesta realitze un commit. Si aquesta altra transacció fallara abans de realitzar el commit es faria un rollback i la 1a transacció estaria llegint valors incorrectes (**dirty reads**).
- **SERIALIZABLE**. Aquest és el mètode més restrictiu. És similar a REPEATABLE_READ però incorpora la restricció de no permetre que una transacció interferisca amb un altra.

Podem canviar el mode d'aïllament de dades, bé mitjançant la variable de sistema [transaction_isolation](https://dev.mysql.com/doc/refman/8.4/en/server-options.html#option_mysqld_transaction-isolation) o amb la següent [instrucció](https://dev.mysql.com/doc/refman/8.4/en/set-transaction.html):

    SET [GLOBAL|SESSION] TRANSACTION ISOLATION LEVEL mode

## Exercici
Criteris d'avaluació 2b, 2h.

- Comprova el valor que tenen les variables d'InnoDB vistes als apunts (comandament **SHOW**).
- Comprova si el valor es correspon amb el que hauria de tindre segons el manual de referència de MySQL.
- Configurar la grandària del buffer d'InnoDB (**innodb_buffer_pool_size**), perquè siga el 50% de la memòria RAM de la instància EC2.
- Configurar la grandària del registre de transaccions (**innodb_log_file_size**), perquè siga de 256MB.
- Comprova que el valor de les variables modificades ha canviat (comandament **SHOW**).
- Comprova els diferents modes d'aïllament de dades que proporciona MySQL.