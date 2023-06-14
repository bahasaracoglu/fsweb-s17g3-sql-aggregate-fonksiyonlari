# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın.

    1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    SELECT ograd, ogrsoyad, atarih
    FROM ogrenci, islem
    WHERE ogrenci.ogrno = islem.ogrno;

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    SELECT kitapadi, turadi FROM kitap,tur
    WHERE kitap.turno = tur.turno AND tur.turadi IN ('Fıkra', 'Hikaye')

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    SELECT ogrenci.ogrno AS 'Öğrenci Numarası',ograd AS 'Adı', ogrsoyad AS 'Soyadı', kitap.kitapadi AS 'Okuduğu Kitap' FROM ogrenci,islem,kitap
    WHERE ogrenci.ogrno = islem.ogrno AND islem.kitapno=kitap.kitapno AND  ogrenci.sinif IN ('10B','10C')


    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    SELECT ograd, ogrsoyad, i.atarih FROM ogrenci as o
    JOIN islem as i ON o.ogrno = i.ogrno;



    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    SELECT kitap.kitapadi, tur.turadi FROM kitap
    JOIN tur ON kitap.turno = tur.turno
    WHERE tur.turadi IN ('Fıkra', 'Hikaye')


    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    SELECT ogrenci.ogrno ,ogrenci.ograd , ogrenci.ogrsoyad, kitap.kitapadi FROM ogrenci
    JOIN islem ON ogrenci.ogrno = islem.ogrno
    JOIN kitap ON islem.kitapno=kitap.kitapno
    WHERE ogrenci.sinif IN ('10B','10C')
    ORDER BY ogrenci.ograd ASC

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    SELECT ogrenci.ogrno ,ogrenci.ograd , ogrenci.ogrsoyad, islem.atarih FROM ogrenci
    LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
    WHERE ogrenci.sinif IN ('10B','10C')


    8) Kitap almayan öğrencileri listeleyin.

    SELECT ogrenci.ogrno ,ogrenci.ograd , ogrenci.ogrsoyad, islem.islemno FROM ogrenci
    LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
    WHERE islem.islemno IS NULL
    ORDER BY ogrenci.ograd ASC


    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    SELECT kitap.kitapno,kitap.kitapadi, COUNT(islem.kitapno) AS 'Ödünç Alınma Sayısı' FROM kitap
    JOIN islem ON islem.kitapno = kitap.kitapno
    GROUP BY kitap.kitapno
    ORDER BY kitap.kitapno ASC



    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT kitap.kitapno,kitap.kitapadi, COUNT(islem.kitapno) AS 'Ödünç Alınma Sayısı' FROM kitap
    LEFT JOIN islem ON islem.kitapno = kitap.kitapno
    GROUP BY kitap.kitapno
    ORDER BY kitap.kitapno ASC



    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    SELECT ograd, ogrsoyad, k.kitapadi FROM ogrenci as o
    JOIN islem as i ON o.ogrno = i.ogrno
    JOIN kitap as k ON  i.kitapno= k.kitapno


    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    SELECT ograd, ogrsoyad, k.kitapadi,CONCAT(yazarad,' ',yazarsoyad) AS 'Yazar Ad-Soyad' , t.turadi,i.atarih FROM ogrenci as o
    LEFT JOIN islem as i ON o.ogrno = i.ogrno
    LEFT JOIN kitap as k ON  i.kitapno= k.kitapno
    LEFT JOIN yazar as y ON  k.yazarno= y.yazarno
    LEFT JOIN tur as t ON  k.turno= t.turno



    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

    SELECT ograd, ogrsoyad, COUNT(islem.islemno) AS 'Okuduğu Kitap Sayısı' FROM ogrenci
    JOIN islem ON ogrenci.ogrno = islem.ogrno
    JOIN kitap ON islem.kitapno = kitap.kitapno
    WHERE ogrenci.sinif IN ('10A','10B')
    GROUP BY ogrenci.ogrno


    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

    SELECT AVG(sayfasayisi) FROM kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

    SELECT * FROM kitap
    WHERE sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    SELECT COUNT(ogrno) FROM ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    SELECT COUNT(ogrno) AS  'Toplam Sayı' FROM ogrenci


    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    SELECT COUNT(distinct ograd) FROM ogrenci


    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    SELECT MAX(sayfasayisi) FROM kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    SELECT kitapadi, sayfasayisi FROM kitap
    WHERE sayfasayisi = (SELECT MAX(sayfasayisi) from kitap)


    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    SELECT MIN(sayfasayisi) FROM kitap


    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    SELECT kitapadi, sayfasayisi FROM kitap
    WHERE sayfasayisi = (SELECT MIN(sayfasayisi) from kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    SELECT MAX(sayfasayisi) FROM kitap
    INNER JOIN tur ON kitap.turno= tur.turno
    WHERE tur.turadi = 'Dram';


    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    SELECT SUM(sayfasayisi) FROM ogrenci
    JOIN islem ON ogrenci.ogrno = islem.ogrno
    JOIN kitap ON islem.kitapno= kitap.kitapno
    WHERE ogrenci.ogrno = 15



    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    SELECT ograd, COUNT(ogrno) FROM ogrenci
    GROUP BY ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.

    SELECT sinif, COUNT(ogrno) FROM ogrenci
    GROUP BY sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

    SELECT sinif, cinsiyet, COUNT(ogrno) AS ogrenci_sayisi
    FROM ogrenci
    GROUP BY sinif, cinsiyet;


    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    SELECT ogrenci.ograd, ogrenci.ogrsoyad, SUM(kitap.sayfasayisi) AS 'Toplam Sayfa Sayısı' FROM ogrenci
    JOIN islem ON ogrenci.ogrno = islem.ogrno
    JOIN kitap ON islem.kitapno = kitap.kitapno
    GROUP BY ogrenci.ograd, ogrenci.ogrsoyad
    ORDER BY SUM(kitap.sayfasayisi) DESC


    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

    SELECT ogrenci.ograd, ogrenci.ogrsoyad, COUNT(kitap.kitapno) AS 'Okuduğu Kitap Sayısı' FROM ogrenci
    JOIN islem ON ogrenci.ogrno = islem.ogrno
    JOIN kitap ON islem.kitapno = kitap.kitapno
    GROUP BY ogrenci.ogrno,ogrenci.ograd, ogrenci.ogrsoyad
    ORDER BY ogrenci.ograd
