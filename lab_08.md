### Zadanie 1 - Spisek
```sql
CREATE TABLE infs_krupickik.uczestnicy
	SELECT * FROM wikingowie.uczestnicy;
ALTER TABLE uczestnicy 
	ADD PRIMARY KEY(id_wyprawy, id_uczestnika);
ALTER TABLE uczestnicy 
	ADD FOREIGN KEY(id_wyprawy) REFERENCES wyprawa(id_wyprawy);
ALTER TABLE uczestnicy 
	ADD FOREIGN KEY(id_uczestnika) REFERENCES kreatura(idKreatury);
CREATE TABLE infs_krupickik.etapy_wyprawy 
	SELECT * FROM wikingowie.etapy_wyprawy;
ALTER TABLE etapy_wyprawy 
	ADD PRIMARY KEY(idEtapu);
ALTER TABLE etapy_wyprawy 
	ADD FOREIGN KEY(idWyprawy) REFERENCES wyprawa(id_wyprawy)
ALTER TABLE etapy_wyprawy 
	ADD FOREIGN KEY(sektor) REFERENCES sektor(id_sektora);
CREATE TABLE infs_krupickik.sektor 
	SELECT * FROM wikingowie.sektor;
ALTER TABLE sektor 
	ADD PRIMARY KEY(id_sektora);
CREATE TABLE infs_krupickik.wyprawa 
	SELECT * FROM wikingowie.wyprawa;
ALTER TABLE wyprawa 
	ADD PRIMARY KEY (id_wyprawy);
ALTER TABLE wyprawa 
	ADD FOREIGN KEY(kierownik) REFERENCES kreatura(idKreatury) ON DELETE CASCADE;
	
SELECT kreatura.nazwa FROM kreatura 
	LEFT JOIN uczestnicy ON kreatura.idKreatury = uczestnicy.id_uczestnika 
	WHERE uczestnicy.id_uczestnika IS NULL

SELECT wyprawa.nazwa, SUM(ekwipunek.ilosc) FROM wyprawa 
	JOIN uczestnicy ON wyprawa.id_wyprawy = uczestnicy.id_wyprawy 
	JOIN ekwipunek ON uczestnicy.id_uczestnika = ekwipunek.idKreatury 
	GROUP BY wyprawa.nazwa;
```
### Zadanie 2 - Trudna decyzja
```sql
SELECT wyprawa.nazwa, COUNT(id_uczestnika) AS liczba_uczestnikow, GROUP_CONCAT(kreatura.nazwa) AS uczestnicy FROM wyprawa 
	JOIN uczestnicy ON wyprawa.id_wyprawy = uczestnicy.id_wyprawy 
	JOIN kreatura ON uczestnicy.id_uczestnika = kreatura.idKreatury 
	GROUP BY wyprawa.nazwa;
	
SELECT wyprawa.nazwa, etapy_wyprawy.kolejnosc, sektor.nazwa, kreatura.nazwa FROM wyprawa 
	JOIN etapy_wyprawy ON wyprawa.id_wyprawy = etapy_wyprawy.idWyprawy 
	JOIN sektor ON etapy_wyprawy.sektor = sektor.id_sektora 
	JOIN kreatura ON wyprawa.kierownik = kreatura.idKreatury 
	ORDER BY wyprawa.data_rozpoczecia;
```
### Zadanie 3 - Misterny plan
```sql
SELECT sektor.nazwa, sektor.id_sektora, IFNULL(COUNT(etapy_wyprawy.sektor), 0) AS liczba_odwiedzin FROM sektor 
	LEFT JOIN etapy_wyprawy ON etapy_wyprawy.sektor = sektor.id_sektora 
	LEFT JOIN wyprawa ON etapy_wyprawy.idWyprawy = wyprawa.id_wyprawy 
	GROUP BY id_sektora;

SELECT kreatura.nazwa, IF(COUNT(uczestnicy.id_uczestnika)>0, 'bral udzial w wyprawie', 'nie bral udzialu w wyprawie') czy_bral_udzial FROM kreatura 
	LEFT JOIN uczestnicy ON kreatura.idKreatury = uczestnicy.id_uczestnika 
	LEFT JOIN wyprawa ON wyprawa.id_wyprawy = uczestnicy.id_wyprawy 
	GROUP BY kreatura.nazwa;
```
### Zadanie 4 - Pułapka
```sql
SELECT wyprawa.nazwa, SUM(LENGTH(etapy_wyprawy.dziennik)) suma_znakow FROM wyprawa 
	JOIN etapy_wyprawy ON wyprawa.id_wyprawy = etapy_wyprawy.idWyprawy 
	GROUP BY wyprawa.nazwa 
	HAVING suma_znakow < 400;

SELECT w.id_wyprawy, w.nazwa, SUM(z.waga*e.ilosc)/COUNT(DISTINCT u.id_uczestnika) srednia_waga_niesionych_zasobow FROM wyprawa w
	JOIN uczestnicy u ON u.id_wyprawy = w.id_wyprawy
	JOIN ekwipunek e ON e.idKreatury = u.id_uczestnika
	JOIN zasob z ON e.idZasobu = z.idZasobu
	GROUP BY w.id_wyprawy;
```
### Zadanie 5 - Tchórz
```sql
SELECT kreatura.nazwa, DATEDIFF(wyprawa.data_rozpoczecia, kreatura.dataUr) FROM kreatura 
	JOIN uczestnicy ON uczestnicy.id_uczestnika = kreatura.idKreatury 
	JOIN wyprawa ON wyprawa.id_wyprawy = uczestnicy.id_wyprawy 
	JOIN etapy_wyprawy ON wyprawa.id_wyprawy = etapy_wyprawy.idWyprawy 
	JOIN sektor ON etapy_wyprawy.sektor = sektor.id_sektora 
	WHERE sektor.nazwa = 'Chatka Dziadka';
```
