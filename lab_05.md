### Zadanie 1 - CICHA WODA
```sql
DELETE FROM postac WHERE nazwa <> 'Bjorn' AND rodzaj = 'wiking' ORDER BY wiek DESC LIMIT 2;

ALTER TABLE walizka DROP CONSTRAINT walizka_ibfk_1;
ALTER TABLE przetwory DROP CONSTRAINT przetwory_ibfk_1;
ALTER TABLE przetwory DROP CONSTRAINT przetwory_ibfk_2;
ALTER TABLE postac MODIFY id_postaci INT;
ALTER TABLE postac DROP PRIMARY KEY;
```

### Zadanie 2 - SYRENKA
```sql
ALTER TABLE postac ADD pesel CHAR(11) FIRST;
UPDATE postac SET pesel = '54290521945' + id_postaci;
ALTER TABLE postac ADD PRIMARY KEY(pesel);

ALTER TABLE postac MODIFY rodzaj ENUM('wiking', 'ptak', 'kobieta', 'syrena');

INSERT INTO postac VALUES ('57941048240', 9, 'Gertruda Nieszczera', 'syrena', '0904-02-13', 38, NULL, NULL);
```
### Zadanie 3 - PRZECHYŁY
```sql
UPDATE postac SET statek = 'Drakkar' WHERE nazwa LIKE '%a%';

SELECT * FROM statek WHERE data_wodowanie >= '1901-01-01' AND data_wodowanie <= '2000-12-31';
lub
SELECT * FROM statek WHERE data_wodowanie BETWEEN '1901-01-01' AND '2000-12-31';
lub
SELECT * FROM statek WHERE YEAR(data_wodowanie) BETWEEN 1901 AND 2000;

UPDATE statek SET max_ladownosc = 0.7 * max_ladownosc 
  WHERE YEAR(data_wodowanie) BETWEEN 1901 AND 2000;

ALTER TABLE postac 
  MODIFY wiek INT UNSIGNED CHECK (wiek < 1000);

```
### Zadanie 4 - WĄŻ LOKO
```sql
ALTER TABLE postac 
  MODIFY rodzaj ENUM('wiking', 'ptak', 'kobieta', 'syrena', 'waz');
INSERT INTO postac VALUES('34781239524', 10, 'Loko', 'waz', '1023-01-29', 23, NULL, NULL);

-- I sposob (kopiuje klucze glowne, ale nie obce)
CREATE TABLE marynarz LIKE postac;
ALTER TABLE marynarz 
  ADD FOREIGN KEY (statek) REFERENCES statek(nazwa_statku);
INSERT INTO marynarz 
  SELECT * FROM postac WHERE statek IS NOT NULL;
-- II sposob (nie kopiuje kluczy)
CREATE TABLE marynarz2 
  AS SELECT * FROM postac WHERE statek IS NOT NULL;
ALTER TABLE marynarz2 
  ADD PRIMARY KEY pesel;
ALTER TABLE marynarz2 
  ADD FOREIGN KEY (statek) REFERENCES statek(nazwa_statku);
```
### Zadanie 5 - SZTORM
```sql
UPDATE postac SET statek = NULL;

DELETE FROM postac WHERE rodzaj = 'wiking' AND id_postaci = 8;

TRUNCAETE statek;

DROP TABLE statek;

CREATE TABLE zwierz(id INT AUTO_INCREMENT PRIMARY KEY,
  nazwa VARCHAR(100),
  wiek INT);

INSERT INTO zwierz(nazwa, wiek) SELECT nazwa, wiek FROM postac WHERE rodzaj IN ('ptak', 'waz');
```
