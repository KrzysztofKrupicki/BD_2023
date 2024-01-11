```sql
-- 1
SELECT dzial.nazwa, MIN(pracownik.pensja), MAX(pracownik.pensja), AVG(pracownik.pensja) FROM dzial 
	JOIN pracownik ON dzial.id_dzialu = pracownik.dzial 
	GROUP BY dzial.id_dzialu;
-- 2
SELECT klient.pelna_nazwa, SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) AS wartosc FROM klient 
	JOIN zamowienie ON klient.id_klienta = zamowienie.klient 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie 
	GROUP BY klient.id_klienta 
	ORDER BY wartosc DESC LIMIT 10;
-- 3
SELECT YEAR(zamowienie.data_zamowienia) AS rok, SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) AS wartosc FROM zamowienie 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie 
	GROUP BY rok 
	ORDER BY wartosc DESC;
-- 4
SELECT COUNT(zamowienie.id_zamowienia), SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) AS wartosc FROM zamowienie 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie 
	JOIN status_zamowienia ON zamowienie.status_zamowienia = status_zamowienia.id_statusu_zamowienia 
	WHERE status_zamowienia.nazwa_statusu_zamowienia = 'anulowane';
-- 5
SELECT adres_klienta.miejscowosc, COUNT(zamowienie.id_zamowienia) AS ilosc_zamowien, SUM(pozycja_zamowiosc * pozycja_zamowienia.cena) AS wartosc_zamowien FROM adres_klienta 
	JOIN klient ON adres_klienta.klient = klient.id_klienta 
	JOIN zamowienie ON klient.id_klienta = zamowienie.klient 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie 
	GROUP BY adres_klienta.miejscowosc;
-- 6
SELECT SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) AS wartosc FROM pozycja_zamowienia 
	JOIN zamowienie ON pozycja_zamowienia.zamowienie = zamowienie.id_zamowienia 
	JOIN status_zamowienia ON zamowienie.status_zamowienia = status_zamowienia.id_statusu_zamowienia 
	WHERE status_zamowienia.nazwa_statusu_zamowienia = 'zrealizowane';
-- 7
SELECT YEAR(zamowienie.data_zamowienia) AS rok, (SUM(pozycja_zamowienia.ilosc * pozycja_zamowienia.cena) - SUM(towar.cena_zakupu * pozycja_zamowienia.ilosc)) AS przychod FROM zamowienie 
	JOIN pozycja_zamowienia ON zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie 
	JOIN towar ON pozycja_zamowienia.towar = towar.id_towaru 
	GROUP BY rok;
-- 8
SELECT kategoria.nazwa_kategori, SUM(towar.cena_zakupu * stan_magazynowy.ilosc) AS wartosc_stanu_magazynowego FROM kategoria 
	JOIN towar ON kategoria.id_kategori = towar.kategoria 
	JOIN stan_magazynowy ON towar.id_towaru = stan_magazynowy.towar 
	GROUP BY kategoria.id_kategori;
-- 9
-- 10
```
