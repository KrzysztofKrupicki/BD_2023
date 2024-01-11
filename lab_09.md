### Zadanie 1 - Dziura
```sql
DELIMITER //
CREATE TRIGGER kreatura_before_insert
BEFORE INSERT ON kreatura
FOR EACH ROW
BEGIN
IF NEW.waga <= 0
THEN
SET NEW.waga = 1;
END IF;
END
//
DELIMITER ;

DELIMITER //
CREATE TRIGGER kreatura_before_update
BEFORE UPDATE ON kreatura
FOR EACH ROW
BEGIN
IF NEW.waga <= 0
THEN
SET NEW.waga = 1;
END IF;
END
//
DELIMITER ;
```
### Zadanie 2 - Futro
```sql
CREATE TABLE archiwum_wypraw LIKE wyprawa;
ALTER TABLE archiwum_wypraw MODIFY kierownik VARCHAR(200);

DELIMITER //
CREATE TRIGGER wyprawa_before_delete
BEFORE DELETE on wyprawa
FOR EACH ROW
BEGIN
INSERT INTO archiwum_wypraw SELECT wyprawa.id_wyprawy, wyprawa.nazwa, wyprawa.data_rozpoczecia, wyprawa.data_zakonczenia, kreatura.nazwa FROM wyprawa JOIN kreatura ON wyprawa.kierownik = kreatura.idKreatury WHERE wyprawa.id_wyprawy = OLD.id_wyprawy;
END
//
DELIMITER ;
```
### Zadanie 3 - Spalony
```sql
DELIMITER $$
CREATE PROCEDURE eliksir_sily(IN id int)
BEGIN 
UPDATE kreatura SET udzwig = udzwig*1.2 WHERE idKreatury = id;
END
$$
DELIMITER ;
CALL eliksir_sily(100);

DELIMITER $$
CREATE FUNCTION powieksz_tekst(tekst VARCHAR(255)) 
RETURNS VARCHAR(255)
BEGIN 
DECLARE wynik VARCHAR(255);
SET wynik = UPPER(tekst);
RETURN wynik;
END
$$
DELIMITER ;
SELECT powieksz_tekst('hello world');
```
### Zadanie 4 - Salwa śmiechu
```sql
CREATE TABLE system_alarmowy(id_alarmu INT AUTO_INCREMENT PRIMARY KEY, wiadomosc VARCHAR(255));

DELIMITER $$
CREATE TRIGGER tesciowa_nadchodzi
AFTER INSERT on wyprawa
FOR EACH ROW
BEGIN
DECLARE tesciowa varchar(100);
DECLARE sektor_id integer;
DECLARE czy_tesciowa bool;
DECLARE czy_chata_dziadka bool;
SET tesciowa = 'Tesciowa';
SET sektor_id = 7;
INSERT INTO system_alarmowy VALUES (NULL, 'Teściowa nadchodzi');
IF tesciowa in (SELECT nazwa from kreatura WHERE idKreatury in (SELECT id_uczestnika from uczestnicy where id_wyprawy=NEW.id_wyprawy)) 
THEN 
SET czy_tesciowa = true;
END IF;
IF sektor_id in (SELECT sektor FROM etapy_wyprawy WHERE idWyprawy=NEW.id_wyprawy) 
THEN 
SET czy_chata_dziadka = true;
END IF;
IF czy_tesciowa AND czy_chata_dziadka
THEN  
INSERT INTO system_alarmowy VALUES(NULL,'Tesciowa nadchodzi');
END IF;
END
$$
DELIMITER ;
```
### Zadanie 5 - Happy End
```sql
DELIMITER $$
CREATE PROCEDURE avg_suma_max_udzwig_kreatura(OUT avg DECIMAL(10,2), OUT suma DECIMAL(10,2), OUT maks DECIMAL(10,2))
BEGIN 
SELECT AVG(udzwig), SUM(udzwig), MAX(udzwig) FROM kreatura;
END
$$
DELIMITER ;
CALL avg_suma_max_udzwig_kreatura(@avg, @suma, @maks);

DELIMITER $$
CREATE PROCEDURE wyprawa_sektor(IN id_w INT, OUT nazwa_sektora VARCHAR(255))
BEGIN 
SELECT CONCAT(sektor.wspolrzedna_x, wspolrzedna_y) FROM wyprawa JOIN etapy_wyprawy ON wyprawa.id_wyprawy = etapy_wyprawy.idWyprawy JOIN sektor ON etapy_wyprawy.sektor = sektor.id_sektora WHERE wyprawa.id_wyprawy = id_w ORDER BY etapy_wyprawy.idEtapu;
END
$$
DELIMITER ;
CALL wyprawa_sektor(2, @nazwa_sektora);
```
