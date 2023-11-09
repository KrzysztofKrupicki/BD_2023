### Zadanie lab_04 - 9.11.2023
```sql
CREATE TABLE postac(id_postaci INT PRIMARY KEY AUTO_INCREMENT,
nazwa VARCHAR(40),
rodzaj ENUM('wiking', 'ptak', 'kobieta'),
data_ur DATE,
wiek INT UNSIGNED);
DESC postac;
```
| Field      | Type                            | Null | Key | Default | Extra          |
|------------|---------------------------------|------|-----|---------|----------------|
| id_postaci | int                             | NO   | PRI | NULL    | auto_increment |
| nazwa      | varchar(40)                     | YES  |     | NULL    |                |
| rodzaj     | enum('wiking','ptak','kobieta') | YES  |     | NULL    |                |
| data_ur    | date                            | YES  |     | NULL    |                |
| wiek       | int unsigned                    | YES  |     | NULL    |                |
```sql
INSERT INTO postac VALUES (NULL, 'Bjorn', 'wiking', '1971-01-12', YEAR(NOW())-1971),
(NULL, 'Drozd', 'ptak', '2021-10-25', YEAR(NOW())-2021),
(NULL, 'Tesciowa', 'kobieta', '1957-05-07', YEAR(NOW())-1957);
SELECT * FROM postac;
```
| id_postaci | nazwa    | rodzaj  | data_ur    | wiek |
|------------|----------|---------|------------|------|
|          1 | Bjorn    | wiking  | 1971-01-12 |   52 |
|          2 | Drozd    | ptak    | 2021-10-25 |    2 |
|          3 | Tesciowa | kobieta | 1957-05-07 |   66 |
```sql
UPDATE postac SET wiek = 88 WHERE nazwa = 'Tesciowa';
SELECT * FROM postac;
```
| id_postaci | nazwa    | rodzaj  | data_ur    | wiek |
|------------|----------|---------|------------|------|
|          1 | Bjorn    | wiking  | 1971-01-12 |   52 |
|          2 | Drozd    | ptak    | 2021-10-25 |    2 |
|          3 | Tesciowa | kobieta | 1957-05-07 |   88 |
```sql
CREATE TABLE walizka(id_walizki INT PRIMARY KEY AUTO_INCREMENT,
pojemnosc INT UNSIGNED,
kolor ENUM('rozowy', 'czerwony', 'teczowy', 'zolty'),
id_wlasciciela INT,
FOREIGN KEY(id_wlasciciela) REFERENCES postac(id_postaci) ON DELETE CASCADE);
DESC walizka;
```
| Field          | Type                                        | Null | Key | Default | Extra          |
|----------------|---------------------------------------------|------|-----|---------|----------------|
| id_walizki     | int                                         | NO   | PRI | NULL    | auto_increment |
| pojemnosc      | int unsigned                                | YES  |     | NULL    |                |
| kolor          | enum('rozowy','czerwony','teczowy','zolty') | YES  |     | NULL    |                |
| id_wlasciciela | int                                         | YES  | MUL | NULL    |                |
```sql
ALTER TABLE walizka
ALTER COLUMN kolor SET DEFAULT 'rozowy';
DESC walizka
```
| Field          | Type                                        | Null | Key | Default | Extra          |
|----------------|---------------------------------------------|------|-----|---------|----------------|
| id_walizki     | int                                         | NO   | PRI | NULL    | auto_increment |
| pojemnosc      | int unsigned                                | YES  |     | NULL    |                |
| kolor          | enum('rozowy','czerwony','teczowy','zolty') | YES  |     | rozowy  |                |
| id_wlasciciela | int                                         | YES  | MUL | NULL    |                |
```sql
INSERT INTO walizka VALUES (NULL, 30, 'czerwony', 1);
INSERT INTO walizka(pojemnosc, id_wlasciciela) VALUES (50, 3);
SELECT * FROM walizka
```
| id_walizki | pojemnosc | kolor    | id_wlasciciela |
|------------|-----------|----------|----------------|
|          1 |        30 | czerwony |              1 |
|          2 |        50 | rozowy   |              3 |
```sql
CREATE TABLE izba(adres_budynku VARCHAR(255),
nazwa_izby VARCHAR(255),
metraz INT UNSIGNED,
wlasciciel INT,
PRIMARY KEY(adres_budynku, nazwa_izby),
FOREIGN KEY (wlasciciel) REFERENCES postac(id_postaci) ON DELETE SET NULL);
DESC izba;
```
| Field         | Type         | Null | Key | Default | Extra |
|---------------|--------------|------|-----|---------|-------|
| adres_budynku | varchar(255) | NO   | PRI | NULL    |       |
| nazwa_izby    | varchar(255) | NO   | PRI | NULL    |       |
| metraz        | int unsigned | YES  |     | NULL    |       |
| wlasciciel    | int          | YES  | MUL | NULL    |       |
```sql
ALTER TABLE izba ADD COLUMN kolor_izby VARCHAR(255) DEFAULT 'czarny' AFTER metraz;
DESC izba;
```
| Field         | Type         | Null | Key | Default | Extra |
|---------------|--------------|------|-----|---------|-------|
| adres_budynku | varchar(255) | NO   | PRI | NULL    |       |
| nazwa_izby    | varchar(255) | NO   | PRI | NULL    |       |
| metraz        | int unsigned | YES  |     | NULL    |       |
| kolor_izby    | varchar(255) | YES  |     | czarny  |       |
| wlasciciel    | int          | YES  | MUL | NULL    |       |
```sql
INSERT INTO izba(adres_budynku, nazwa_izby) VALUES ('Valhalla 9, Asgard', 'Hala Mocy');
SELECT * FROM izba;
```
| adres_budynku      | nazwa_izby | metraz | kolor_izby | wlasciciel |
|--------------------|------------|--------|------------|------------|
| Valhalla 9, Asgard | Hala Mocy  |   NULL | czarny     |       NULL |

