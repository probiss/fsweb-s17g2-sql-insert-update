# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosuna KEMAL UYUMAZ isimli yazarı ekleyin.

		insert into yazar(yazarad,yazarsoyad) values('Kemal','UYUMAZ')
	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	
		insert into tur values('Biyografi')
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 

		insert into ogrenci(ograd,ogrsoyad,sinif,cinsiyet)
		values('Çağlar','Üzümcü','10A','E'),('Leyla','Alagöz','9B','K'),				 
		('Ayşe','Bektaş','11C','K')

	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

		insert into yazar(yazarad, yazarsoyad)
		select ograd,ogrsoyad from ogrenci 
		order by rand() limit 1
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

		insert into yazar (yazarad, yazarsoyad) select ograd, ogrsoyad where ogrno between 10 and 30
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	
		insert into yazar (yazarad, yazarsoyad) values ("Nurettin", "BELEK")
		select * yazar where yazarno = @@IDENTITY

	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

		update ogrenci set sinif = "10C" where ogrno = "3"
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın

		update ogrenci set sinif = "10A" where sinif = "9A"
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.

		update ogrenci set puan = puan + 5 where puan is not null

	10) 25 numaralı yazarı silin.

		delete from yazar where yazarno = 25

	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)

		select * from ogrenci where dtarih is null
	
	12) Doğum tarihi null olan öğrencileri silin. 

		delete from ogrenci where dtarih is null
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

		update kitap set puan = puan + 2 where kitapadi like "a%"
	
	14) Kişisel Gelişim isimli bir tür oluşturun.

		insert into tur (turadi) values ("Kişisel Gelişim")
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
	
		update kitap set turno = @@IDENTITY where kitapadi = "Başarı Rehberi"

	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.

		create procedure ogrencilistesi as select * from ogrenci; exec ogrencilistesi
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.

		create procedure ekle @fname nvarchar(50), @lname nvarchar(50),@class nvarchar(3), @sex nvarchar(1) as 				insert into ogrenci (ograd, ogrsoyad, sinif, cinsiyet) 
		values (@fname, @lname, @class, @sex)

		exec ekle "Cihat", "BULUT", "10E", "E";
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

		create procedure sil @id numeric(10) as delete from ogrenci where ogrno = @id

		exec sil ....
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

		create procedure changeclass @id numeric(10), @class nvarchar(3) as update ogrenci 
		set sinif = @class where ogrno = @id

		exec changeclass ... , ...
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.

		create procedure birlestir as select concat(ograd, ogrsoyad) as adsoyad from ogrenci

		exec birlestir
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.

		drop procedure [procedures]
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.

		select tur.*, kitap.* from tur as t, kitap as ktp
		where t.turno = (select t.turno from t where t.turadi="Dram")
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.

		select kitap.*, yazar.* from kitap as ktp, yazar as y
		where y.yazarno in(select y.yazarno from y where y.yazarad like "e%")
	
	24) Kitap okumayan öğrencileri listeleyiniz.

		select * from ogrenci where ogrenci.ogrno not in ( select distinct islem.ogrno from islem)
	
	25) Okunmayan kitapları listeleyiniz

		select * from kitap where kitap.kitapno not in (select distinct islem.kitapno from islem)
	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.

		select * from kitap where kitap.kitapno not in (select distinct islem.kitapno from islem where 
		MONTH(islem.atarih)=5)
 
