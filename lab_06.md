### Zadanie 1 - Rozbitek
```sql
CREATE TABLE infs_krupickik.kreatura 
  SELECT * FROM wikingowie.kreatura;
ALTER TABLE kreatura 
  ADD PRIMARY KEY(idKreatury);
CREATE TABLE infs_krupickik.zasob 
  SELECT * FROM wikingowie.zasob;
ALTER TABLE zasob 
  ADD PRIMARY KEY(zasob);
CREATE TABLE infs_krupickik.ekwipunek 
  SELECT * FROM wikingowie.ekwipunek;
ALTER TABLE ekwipunek 
  ADD PRIMARY KEY(idEkwipunku);
ALTER TABLE ekwipunek 
  ADD FOREIGN KEY (idKreatury) REFERENCES kreatura(idKreatury);
ALTER TABLE ekwipunek 
  ADD FOREIGN KEY (idZasobu) REFERENCES zasob(idZasobu);

SELECT * FROM zasob;

SELECT * FROM zasob 
  WHERE rodzaj = 'jedzenie';

SELECT idZasobu, ilosc FROM ekwipunek 
  WHERE idKreatury IN (1,3,5);
```
### Zadanie 2 - Kokos?
```sql
SELECT * FROM kreatura 
  WHERE rodzaj <> 'wiedzma' AND udzwig >= 50;

SELECT * FROM zasob 
  WHERE waga BETWEEN 2 AND 5;

SELECT * FROM kreatura 
  WHERE (nazwa LIKE '%or%') AND (udzwig BETWEEN 30 AND 70);
```
### Zadanie 3 - Hamak
```sql
SELECT * FROM zasob 
  WHERE MONTH(dataPozyskania) IN(7,8);

SELECT * FROM zasob 
  WHERE rodzaj IS NOT NULL 
  ORDER BY waga ASC;

SELECT * FROM kreatura 
  WHERE dataUr IS NOT NULL 
  ORDER BY dataUr ASC LIMIT 5;
```
### Zadanie 4 - Złota rybka
```sql
SELECT DISTINCT rodzaj FROM zasob 
  WHERE rodzaj IS NOT NULL;

SELECT CONCAT(nazwa,' - ',rodzaj) AS 'nazwa - rodzaj' FROM kreatura 
  WHERE rodzaj LIKE 'wi%';

SELECT nazwa, ilosc*waga AS 'Calkowita waga' FROM zasob 
  WHERE YEAR(dataPozyskania) BETWEEN 2000 AND 2007;
```
### Zadanie 5 - Twardy sen
```sql
SELECT nazwa, waga*ilosc*0.7 AS 'Netto', waga*ilosc*0.3 AS 'Odpadu' FROM zasob 
  WHERE rodzaj = 'jedzenie';

SELECT * FROM zasob WHERE rodzaj IS NULL;

SELECT DISTINCT rodzaj FROM zasob WHERE nazwa LIKE 'Ba%' OR nazwa LIKE '%os';
```
