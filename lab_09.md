### Zadanie 1 - Dziura
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

### Zadanie 2 - Futro
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

### Zadanie 3 - Spalony
DELIMITER $$
CREATE PROCEDURE eliksir_sily(IN id int)
BEGIN 
UPDATE kreatura SET udzwig = udzwig*1.2 WHERE idKreatury = id;
END
$$
DELIMITER ;
CALL eliksir_sily(100);

DELIMITER $$
CREATE PROCEDURE powieksz_tekst(IN tekst VARCHAR(255))
BEGIN 
SELECT UPPER(tekst) powieksz_tekst;
END
$$
DELIMITER ;
CALL powieksz_tekst('hello world');

### Zadanie 4 - Salwa śmiechu
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

### Zadanie 5 - Happy End
