# Deney Sonu Teslimatı

Sistem Programlama ve Veri Yapıları bakış açısıyla veri tabanlarındaki performansı öne çıkaran hususlar nelerdir?

Aşağıda kutucuk (checkbox) ile gösterilen maddelerden en az birini seçtiğiniz açık kaynak kodlu bir VT kaynak kodları üzerinde göstererek açıklayınız. Açıklama bölümüne kısaca metninizi yazıp, kod üzerinde gösterim videonuzun linkini en altta belirtilen kutucuğa yerleştiriniz.

- [X]  Seçtiğiniz konu/konuları bu şekilde işaretleyiniz. **!**
    
---

# 1. Sistem Perspektifi (Operating System, Disk, Input/Output)

### Disk Erişimi

- [ ]  **Blok bazlı disk erişimi** → block_id + offset
- [ ]  Rastgele erişim

### VT için Page (Sayfa) Anlamı

- [ ]  VT hangisini kullanır? **Satır/ Sayfa** okuması

---

### Buffer Pool

- [x]  Veritabanları, Sık kullanılan sayfaları bellekte (RAM) kopyalar mı (caching) ?

- [x]  LRU / CLOCK gibi algoritmaları
- [x]  Diske yapılan I/O nasıl minimize ederler?

# 2. Veri Yapıları Perspektifi

- [ ]  B+ Tree Veri Yapıları VT' lerde nasıl kullanılır?
- [ ]  VT' lerde hangi veri yapıları hangi amaçlarla kullanılır?
- [ ]  Clustered vs Non-Clustered Index Kavramı
- [ ]  InnoDB satırı diskte nasıl durur?
- [ ]  LSM-tree (LevelDB, RocksDB) farkı
- [ ]  PostgreSQL heap + index ayrımı

DB diske yazarken:

- [ ]  WAL (Write Ahead Log) İlkesi
- [ ]  Log disk (fsync vs write) sistem çağrıları farkı

---

# Özet Tablo

| Kavram      | Bellek          | Disk / DB      |
| ----------- | --------------- | -------------- |
| Adresleme   | Pointer         | Page + Offset  |
| Hız         | O(1)            | Page IO        |
| PK          | Yok             | Index anahtarı |
| Veri yapısı | Array / Pointer | B+Tree         |
| Cache       | CPU cache       | Buffer Pool    |

---

# Video [Linki](https://www.youtube.com/watch?v=Nw1OvCtKPII&t=2635s) 
Ekran kaydı. 2-3 dk. açık kaynak V.T. kodu üzerinde konunun gösterimi. Video kendini tanıtma ile başlamalıdır (Numara, İsim, Soyisim, Teknik İlgi Alanları). 

---

# Açıklama (Ort. 300 kelime)

 Disk erişim maliyeti veri tabanı performanslarını belirleyen etkenlerden biridir. CPU ve RAM erişimleri nanosaniyeler şeklindeyken disk erişimleri milisaniyeler şeklindedir. Bundan dolayı veri tabanı yönetim sistemleri performansı arttırmak için RAM üzerinde çalışacak şekilde oluşturulmuştur. Bu yapılar içerinde bulunan bileşenlerden biri de Buffer Pool yapısıdır. 
 
 Buffer Pool yapısı, veritabanında disk üzerindeki veri sayfalarını kopyalarını RAM'de tutan bellek alanıdır. Sorgu sırasında veritabanı önce veri sayfasının buffer pool içerisinde bulunup bulunmadığını kontrol eder. Bellekte mevcutsa veri RAM üzerinden okunur.Bu yapı sayesinde caching mekanizması oluşturulur. Veritabanları diskten veri okurken satır bazlı değil, sayfa bazlı okuma yapar. Yani tek bir disk erişimiyle birden fazla satır RAM’e alınır. Bu sayede aynı sayfa içerisindeki farklı satırlara erişim gerektiğinde tekrar disk erişimi yapılmasına gerek kalmaz. 
 
 Buffer Pool yapısında sınırlı bir bellek alanı olduğundan dolayı yeni sayfalar eklendiğinde bazı sayfalar bellekten çıkarılmalıdır. Bunun için PostgeSQL'de LRU algoritmasına benzer şekilde çalışan CLOCK algoritması kullanılır. LRU algoritması, bellek yönetiminde kullanılan sayfa değiştirme algoritmasıdır. Amacı bellek dolu olduğunda hangi veri sayfasının çıkarılacağına karar vermektir. Aynı şekilde CLOCK algoritmasıda aynı işlevi görür ve uzun süre erişilmeyen sayfalar bellekten çıkarılırken sürekli kullanılan sayfalar bellekte kalmaya devam eder.CLOCK algoritmasında her sayfa için bir kullanım biti tutulur ve dairesel bir yapı üzerinde çalışan bir işaretçi ile sayfalar kontrol edilir. CLOCK algoritması, LRU algoritmasına göre daha az maliyetli olduğundan dolayı tercih edilmektedir.
 
 Buffer Pool yapısı sayesinde verinin önce RAM'de bulunuyorsa diske uğramadan RAM üzerinden veriye ulaşması sayesinde bellekte input, output işlemleri minimize edilir. Bu sayede hem disk yükü azalır hemde sorgu süresi azalır. Disk erişiminin azalması, CPU'nun bekleme süresi azaltılır bu sayede daha verimli ve performansı yüksek bir sistem elde edilir.
 
 Sonuç olarak Buffer Pool yapısı sayesinde disk erişimi azaltılarak yüksek performanslı bir sistem oluşur. Büyük veri setleriyle uğraşıldığında buffer pool yapısı sayesinde performans açısından önemlidir. Veritabanları daha hızlı, verimli ve ölçeklenebilir bir şekilde çalışabilir.

## VT Üzerinde Gösterilen Kaynak Kodları

PostgreSQL'de buffer pool yapısının ana tanımı ve veri yapılarının yer aldığı dosya ve paylaşımlı bellek üzerindeki buffer yönetimi:   [Linki](https://github.com/postgres/postgres/blob/master/src/backend/storage/buffer/bufmgr.c) \
Buffer pool içerisinde kullanılan buffer descriptor yapılarının ve buffer durum bilgilerinin tanımlandığı başlık dosyası ve sayfaların bellekteki durumları hakkında kullanım bilgilerinin takip edilmesi: [Linki](https://github.com/postgres/postgres/blob/master/src/include/storage/buf_internals.h) \
PostgreSQL'de CLOCK algoritmasına benzer şekilde çalışan buffer replacement mekanizmasının uygulandığı kaynak kod dosyası: [Linki](https://github.com/postgres/postgres/blob/master/src/backend/storage/buffer/freelist.c) \
... \
...
