# Esquemes a MySQL
MySQL inclou una sèrie d'esquemes per defecte que defineixen alguns conceptes importants del SGBD.

## Diccionari de dades
És un component essencial en qualsevol SGBD perquè conté informació (metadades) sobre els objectes allotjats en el nostre servidor.

En MySQL s'emmagatzemen en la base de dades **information_schema**. Es poden consultar les dades a través de SELECT, tenint equivalència a alguns comandaments SHOW vist anteriorment.

Les taules d'information_schema es poden consultar a [https://dev.mysql.com/doc/refman/8.4/en/information-schema.html](https://dev.mysql.com/doc/refman/8.4/en/information-schema.html).

## Esquema de rendiment
L'esquema de rendiment de MySQL (**performance_schema**) és una característica per supervisar l'execució del servidor MySQL a baix nivell durant el temps d'execució. Quan podem utilitzar l'esquema de rendiment per supervisar una mètrica concreta, diem que MySQL té instrumentació. Per exemple, els instruments Performance Schema poden proporcionar el nombre d'usuaris connectats:

     mysql> SELECT * FROM performance_schema.users;

Les dades es recopilen gràcies al motor d'emmagatzament **performance_schema**.

Podeu consultar més informació sobre aquest esquema a [https://dev.mysql.com/doc/refman/8.4/en/performance-schema.html](https://dev.mysql.com/doc/refman/8.4/en/performance-schema.html).

## Esquema sys
Des de la versió 5.7, MySQL inclou un esquema complementari per a les dades de performance_schema anomenat sys schema. Aquest esquema consta només de vistes i rutines emmagatzemades de performance_schema. Tot i que està dissenyat per facilitar l'experiència sobre performance_schema, no emmagatzema cap dada per si mateix.

Podeu consultar més informació sobre aquest esquema a [https://dev.mysql.com/doc/refman/8.4/en/sys-schema.html](https://dev.mysql.com/doc/refman/8.4/en/sys-schema.html).

## Esquema mysql
L'esquema del sistema **mysql** conté la informació del servidor MySQL mentre s'executa. Per exemple, inclou informació sobre usuaris i els seus privilegis, taules de zones horàries i rèplica.

Podeu consultar més informació sobre aquest esquema a [https://dev.mysql.com/doc/refman/8.4/en/system-schema.html](https://dev.mysql.com/doc/refman/8.4/en/system-schema.html).


## Exercici
Criteris d'avaluació 2f, 2h.

- Investiga les taules que hi ha a cadascú dels esquemes vists anteriorment.
- Realitza dos SELECT a cada esquema i interpreta els resultats obtinguts.