```sql
-- 1
SELECT imie, nazwisko, YEAR(data_urodzenia) FROM pracownik;
-- 2
SELECT imie, nazwisko, YEAR(DATE(NOW())) - YEAR(data_urodzenia) AS wiek FROM pracownik;
-- 3
SELECT dzial.nazwa, COUNT(pracownik.id_pracownika) AS liczba_pracownikow FROM dzial 
	JOIN pracownik ON dzial.id_dzialu = pracownik.dzial 
	GROUP BY dzial.id_dzialu;
-- 4
SELECT kategoria.nazwa_kategori, COUNT(towar.id_towaru) AS ilosc_produktow FROM kategoria 
	JOIN towar ON kategoria.id_kategori = towar.kategoria 
	GROUP BY kategoria.id_kategori;
-- 5
SELECT kategoria.nazwa_kategori, GROUP_CONCAT(towar.nazwa_towaru, '') FROM kategoria 
	JOIN towar ON kategoria.id_kategori = towar.kategoria 
	GROUP BY kategoria.id_kategori;
-- 6
SELECT ROUND(AVG(pensja), 2) FROM pracownik;
-- 7
SELECT ROUND(AVG(pensja), 2) FROM pracownik WHERE DATEDIFF(DATE(NOW()), data_zatrudnienia)/365.25 >= 5; 
--SELECT ROUND(AVG(pensja), 2) FROM pracownik 
--	WHERE data_zatrudnienia <= '2019-01-11';
--SELECT ROUND(AVG(pensja), 2) FROM pracownik 
--	WHERE SUBDATE(data_zatrudnienia, interval 5 year);
-- 
-- 8
SELECT towar.nazwa_towaru, COUNT(pozycja_zamowienia.id_pozycji) AS ilosc_zamowien FROM towar 
	JOIN pozycja_zamowienia ON towar.id_towaru = pozycja_zamowienia.towar 
	GROUP BY towar.id_towaru 
	ORDER BY ilosc_zamowien DESC LIMIT 10;
--SELECT towar.nazwa_towaru, SUM(pozycja_zamowienia.ilosc) AS suma FROM towar 
--	JOIN pozycja_zamowienia ON towar.id_towaru = pozycja_zamowienia.towar 
--	GROUP BY towar.id_towaru 
--	ORDER BY suma DESC LIMIT 10;
-- 9
SELECT zamowienie.numer_zamowienia, SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) AS wartosc_zamowienia FROM zamowienie 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.id_pozycji 
	WHERE quarter(data_zamowienia) = 1 and year(data_zamowienia) = 2017
	GROUP BY zamowienie.id_zamowienia;
-- 10
SELECT pracownik.imie, pracownik.nazwisko, SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) AS wartosc_zamowien FROM pracownik 
	JOIN zamowienie ON pracownik.id_pracownika = zamowienie.pracownik_id_pracownika 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie 
	GROUP BY pracownik.id_pracownika 
	ORDER BY wartosc_zamowien DESC;
```
