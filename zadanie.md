### Zadanie 1 - LIST
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
### Zadanie 2 - PANIKA
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
  ALTER COLUMN kolor
    SET DEFAULT 'rozowy';
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
### Zadanie 3 - NIESPODZIANKA
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
ALTER TABLE izba
  ADD kolor_izby VARCHAR(255) DEFAULT 'czarny' AFTER metraz;
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
INSERT INTO izba(adres_budynku, nazwa_izby) VALUES ('Valhalla 9, Asgard', 'spizarnia');
SELECT * FROM izba;
```
| adres_budynku      | nazwa_izby | metraz | kolor_izby | wlasciciel 
|--------------------|------------|--------|------------|------------|
| Valhalla 9, Asgard | spizarnia  |   NULL | czarny     |       NULL |
### Zadanie 4 - KOSZMAR
```sql
CREATE TABLE przetwory(
  id_przetworu INT PRIMARY KEY AUTO_INCREMENT,
  rok_produkcji INT DEFAULT 1654,
  id_wykonawcy INT,
  zawartosc VARCHAR(255),
  dodatek VARCHAR(255) DEFAULT 'papryczka chilli',
  id_konsumenta INT,
  FOREIGN KEY (id_wykonawcy) REFERENCES postac(id_postaci),
  FOREIGN KEY (id_konsumenta) REFERENCES postac(id_postaci));
```
| Field         | Type         | Null | Key | Default           | Extra |
|---------------|--------------|------|-----|-------------------|-------|
| id_przetworu  | int          | NO   | PRI | NULL              |       |
| rok_produkcji | int          | YES  |     | 1654              |       |
| id_wykonawcy  | int          | YES  | MUL | NULL              |       |
| zawartosc     | varchar(255) | YES  |     | NULL              |       |
| dodatek       | varchar(255) | YES  |     | papryczka chilli  |       |
| id_konsumenta | int          | YES  | MUL | NULL              |       |
```sql
INSERT INTO przetwory(id_wykonawcy, zawartosc, id_konsumenta) VALUES (1, 'bigos', 3);
SELECT * FROM przetwory;
```
| id_przetworu | rok_produkcji | id_wykonawcy | zawartosc | dodatek          | id_konsumenta |
|--------------|---------------|--------------|-----------|------------------|---------------|
|            1 |          1654 |            1 | bigos     | papryczka chilli |             3 |
### Zadanie 5 - UCIECZKA
```sql
INSERT INTO postac(nazwa, rodzaj, data_ur, wiek) VALUES
('Ragnar Lodbrok', 'wiking', '800-01-01', 40),
('Lagertha', 'wiking', '810-05-15', 35),
('Bjorn Ironside', 'wiking', '830-03-20', 25),
('Ivar the Boneless', 'wiking', '850-11-10', 28),
('Ubbe', 'wiking', '860-07-05', 30);
```
| id_postaci | nazwa             | rodzaj  | data_ur    | wiek |
|------------|-------------------|---------|------------|------|
|          1 | Bjorn             | wiking  | 1971-01-12 |   52 |
|          2 | Drozd             | ptak    | 2021-10-25 |    2 |
|          3 | Tesciowa          | kobieta | 1957-05-07 |   88 |
|          4 | Ragnar Lodbrok    | wiking  | 0800-01-01 |   40 |
|          5 | Lagertha          | wiking  | 0810-05-15 |   35 |
|          6 | Bjorn Ironside    | wiking  | 0830-03-20 |   25 |
|          7 | Ivar the Boneless | wiking  | 0850-11-10 |   28 |
|          8 | Ubbe              | wiking  | 0860-07-05 |   30 |\
```sql
CREATE TABLE statek(
  nazwa_statku VARCHAR(255) PRIMARY KEY,
  rodzaj_statku ENUM('magazynowy', 'handlowy', 'dalekosiezny', 'wojenny', 'transportowy'),
  data_wodowanie DATE,
  max_ladownosc INT UNSIGNED);
```
| Field          | Type                                                                  | Null | Key | Default | Extra |
|----------------|-----------------------------------------------------------------------|------|-----|---------|-------|
| nazwa_statku   | varchar(255)                                                          | NO   | PRI | NULL    |       |
| rodzaj_statku  | enum('magazynowy','handlowy','dalekosiezny','wojenny','transportowy') | YES  |     | NULL    |       |
| data_wodowanie | date                                                                  | YES  |     | NULL    |       |
| max_ladownosc  | int unsigned                                                          | YES  |     | NULL    |       |
```sql
INSERT INTO statek (nazwa_statku, rodzaj_statku, data_wodowanie, max_ladownosc) VALUES
('Drakkar', 'wojenny', '850-01-15', 100),
('Handlowiec', 'handlowy', '820-07-20', 200);
```
| nazwa_statku | rodzaj_statku | data_wodowanie | max_ladownosc |
|--------------|---------------|----------------|---------------|
| Drakkar      | wojenny       | 0850-01-15     |           100 |
| Handlowiec   | handlowy      | 0820-07-20     |           200 |
```sql
ALTER TABLE postac
  ADD funkcja VARCHAR(255);
DESC postac;
```
| Field      | Type                            | Null | Key | Default | Extra          |
|------------|---------------------------------|------|-----|---------|----------------|
| id_postaci | int                             | NO   | PRI | NULL    | auto_increment |
| nazwa      | varchar(40)                     | YES  |     | NULL    |                |
| rodzaj     | enum('wiking','ptak','kobieta') | YES  |     | NULL    |                |
| data_ur    | date                            | YES  |     | NULL    |                |
| wiek       | int unsigned                    | YES  |     | NULL    |                |
| funkcja    | varchar(255)                    | YES  |     | NULL    |                |
```sql
UPDATE postac SET funkcja = 'kapitan' WHERE nazwa = 'Bjorn';
SELECT * FROM postac;
```
| id_postaci | nazwa             | rodzaj  | data_ur    | wiek | funkcja |
|------------|-------------------|---------|------------|------|---------|
|          1 | Bjorn             | wiking  | 1971-01-12 |   52 | kapitan |
|          2 | Drozd             | ptak    | 2021-10-25 |    2 | NULL    |
|          3 | Tesciowa          | kobieta | 1957-05-07 |   88 | NULL    |
|          4 | Ragnar Lodbrok    | wiking  | 0800-01-01 |   40 | NULL    |
|          5 | Lagertha          | wiking  | 0810-05-15 |   35 | NULL    |
|          6 | Bjorn Ironside    | wiking  | 0830-03-20 |   25 | NULL    |
|          7 | Ivar the Boneless | wiking  | 0850-11-10 |   28 | NULL    |
|          8 | Ubbe              | wiking  | 0860-07-05 |   30 | NULL    |
```sql
ALTER TABLE postac
  ADD statek VARCHAR(255);
ALTER TABLE postac
  ADD FOREIGN KEY (statek) REFERENCES statek(nazwa_statku);
```
| Field      | Type                            | Null | Key | Default | Extra          |
|------------|---------------------------------|------|-----|---------|----------------|
| id_postaci | int                             | NO   | PRI | NULL    | auto_increment |
| nazwa      | varchar(40)                     | YES  |     | NULL    |                |
| rodzaj     | enum('wiking','ptak','kobieta') | YES  |     | NULL    |                |
| data_ur    | date                            | YES  |     | NULL    |                |
| wiek       | int unsigned                    | YES  |     | NULL    |                |
| funkcja    | varchar(255)                    | YES  |     | NULL    |                |
| statek     | varchar(255)                    | YES  | MUL | NULL    |                |
```sql
UPDATE postac SET statek = 'Drakkar' WHERE id_postaci IN (1,2,5,7);
UPDATE postac SET statek = 'Handlowiec' WHERE id_postaci IN (4,6,8);
SELECT * FROM postac;
```
| id_postaci | nazwa             | rodzaj  | data_ur    | wiek | funkcja | statek     |
|------------|-------------------|---------|------------|------|---------|------------|
|          1 | Bjorn             | wiking  | 1971-01-12 |   52 | kapitan | Drakkar    |
|          2 | Drozd             | ptak    | 2021-10-25 |    2 | NULL    | Drakkar    |
|          3 | Tesciowa          | kobieta | 1957-05-07 |   88 | NULL    | NULL       |
|          4 | Ragnar Lodbrok    | wiking  | 0800-01-01 |   40 | NULL    | Handlowiec |
|          5 | Lagertha          | wiking  | 0810-05-15 |   35 | NULL    | Drakkar    |
|          6 | Bjorn Ironside    | wiking  | 0830-03-20 |   25 | NULL    | Handlowiec |
|          7 | Ivar the Boneless | wiking  | 0850-11-10 |   28 | NULL    | Drakkar    |
|          8 | Ubbe              | wiking  | 0860-07-05 |   30 | NULL    | Handlowiec |
```sql
DELETE FROM izba WHERE nazwa_izby = 'spizarnia';
DROP TABLE izba;
SHOW TABLES;
```
| Tables_in_infs_krupickik |
|--------------------------|
| postac                   |
| przetwory                |
| statek                   |
| walizka                  |
