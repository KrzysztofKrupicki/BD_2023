### Zadanie 1 - Skwarek
```sql
SELECT AVG(waga) FROM kreatura 
	WHERE rodzaj = 'wiking';

SELECT rodzaj, AVG(waga), COUNT(waga), COUNT(*) FROM kreatura 
	GROUP BY rodzaj;

SELECT rodzaj, AVG(YEAR(DATE(NOW()))-YEAR(dataUr)) FROM kreatura 
	GROUP BY rodzaj;
```
### Zadanie 2 - Dymek
```sql
SELECT rodzaj, SUM(waga*ilosc) FROM zasob 
	GROUP BY rodzaj;

SELECT nazwa, AVG(waga) FROM zasob 
	WHERE ilosc >= 4 
	GROUP BY nazwa 
	HAVING SUM(waga) > 10;

SELECT rodzaj, COUNT(DISTINCT nazwa) FROM zasob 
	GROUP BY rodzaj 
	HAVING COUNT(DISTINCT nazwa) > 1;
```
### Zadanie 3 - Pieprz
```sql
SELECT kreatura.idKreatury, SUM(ekwipunek.ilosc) FROM kreatura 
	JOIN ekwipunek ON kreatura.idKreatury = ekwipunek.idKreatury 
	GROUP BY idKreatury 
	ORDER BY idKreatury;

SELECT kreatura.idKreatury, zasob.nazwa FROM kreatura 
	JOIN ekwipunek ON kreatura.idKreatury = ekwipunek.idKreatury 
	JOIN zasob ON ekwipunek.idZasobu = zasob.idZasobu 
	ORDER BY idKreatury;

SELECT kreatura.idKreatury, ekwipunek.idEkwipunku FROM kreatura 
	LEFT JOIN ekwipunek ON kreatura.idKreatury = ekwipunek.idKreatury 
	WHERE idEkwipunku IS NULL;
```
### Zadanie 4 - Dziadek
```sql
SELECT DISTINCT kreatura.nazwa FROM kreatura 
	JOIN ekwipunek ON kreatura.idKreatury = ekwipunek.idKreatury 
	JOIN zasob ON ekwipunek.idZasobu = zasob.idZasobu 
	WHERE kreatura.rodzaj = 'wiking' AND YEAR(kreatura.dataUr) BETWEEN 1670 AND 1679;

SELECT kreatura.idKreatury, zasob.rodzaj FROM kreatura 
	JOIN ekwipunek ON kreatura.idKreatury  = ekwipunek.idKreatury 
	JOIN zasob ON ekwipunek.idZasobu = zasob.idZasobu 
	WHERE zasob.rodzaj = 'jedzenie' 
	ORDER BY kreatura.dataUr DESC 
	LIMIT 5;

SELECT CONCAT(k1.nazwa,' - ',k2.nazwa) FROM kreatura k1, kreatura k2 
	WHERE k1.idKreatury - 5 = k2.idKreatury OR k1.idKreatury + 5 = k2.idKreatury;
```
### Zadanie 5 - Zła nowina
```sql
SELECT kreatura.rodzaj, AVG(zasob.waga*zasob.ilosc) FROM kreatura 
	JOIN ekwipunek ON kreatura.idKreatury = ekwipunek.idKreatury 
	JOIN zasob ON ekwipunek.idZasobu = zasob.idZasobu 
	WHERE kreatura.rodzaj NOT IN ('malpa', 'waz') AND ekwipunek.ilosc < 30
	GROUP BY kreatura.rodzaj;

SELECT rodzaj, MAX(nazwa), MAX(dataUr), MIN(nazwa), MIN(dataUr)
	FROM kreatura
	GROUP BY rodzaj;
```
