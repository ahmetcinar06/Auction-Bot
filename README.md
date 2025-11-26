# SQLite3 M4L1 — Quick Query Guide

Bu proje ders sırasında kullanılan `data.db` veritabanını içerir. Aşağıda veritabanını bir Query Editor (ör. VS Code SQLite eklentisi) veya `sqlite3` CLI ile açıp çalıştırabileceğiniz hazır SQL sorguları ve kısa talimatlar bulunuyor.

**Gereksinimler**
- `data.db` dosyasının proje kökünde olması gerekir.
- Query editor kullanıyorsanız (VS Code için `SQLite` veya `SQLite Viewer` gibi eklentiler) dosyayı açıp sorgu penceresine SQL yapıştırabilirsiniz.
- İsterseniz Python yüklü ise küçük bir script ile de sorguları çalıştırabilirsiniz.

**Hızlı kullanım (Query Editor)**
1. Query editor ile `data.db` dosyasını açın.
2. Aşağıdaki SQL sorgularını kopyalayıp çalıştırın.

---

1) Listeden en popüler filmi çıkarın (isim ve bütçe):

```sql
SELECT title, budget
FROM movies
ORDER BY popularity DESC
LIMIT 1;
```

2) Aralık 2009'da çıkan en pahalı filmi çıkarın (isim):

```sql
SELECT title
FROM movies
WHERE strftime('%Y-%m', release_date) = '2009-12'
ORDER BY budget DESC
LIMIT 1;
```

3) Sloganı "The battle within" olan filmi çıkarın (isim):

```sql
SELECT title
FROM movies
WHERE LOWER(tagline) = LOWER('The battle within')
LIMIT 1;
```

4) 1980'den önce yayımlanmış ve 8'den fazla oy almış filmleri sıralayın; en fazla oyu alan filmin adını bulun:

```sql
SELECT title, votes
FROM movies
WHERE CAST(strftime('%Y', release_date) AS INTEGER) < 1980
  AND votes > 8
ORDER BY votes DESC;
```

Not: yukarıdaki sorgular `movies`, `title`, `budget`, `popularity`, `release_date`, `tagline`, `votes` sütun adlarını varsayar. Eğer veritabanınızda farklı isimler varsa (ör. `name` yerine `title` yoksa) tablo ve sütun isimlerini sqlite_master veya query editor ile kontrol edip sorguyu ona göre düzeltin.

---

**sqlite3 CLI ile çalıştırma (PowerShell)**

Veritabanı aynı klasördeyse aşağıdaki komutlarla CLI üzerinden sorgu çalıştırabilirsiniz:

```powershell
sqlite3 data.db
-- sqlite> SELECT title, budget FROM movies ORDER BY popularity DESC LIMIT 1;
```

Alternatif olarak tek satırda PowerShell ile çalıştırmak isterseniz:

```powershell
sqlite3 data.db "SELECT title, budget FROM movies ORDER BY popularity DESC LIMIT 1;"
```

**İsteğe bağlı: Python ile çalıştırma**

Eğer bir `main.py` script'iniz varsa veya küçük bir script yazmak isterseniz aşağıdaki yapıyı kullanabilirsiniz:

```python
import sqlite3
conn = sqlite3.connect('data.db')
cur = conn.cursor()
cur.execute("SELECT title, budget FROM movies ORDER BY popularity DESC LIMIT 1;")
print(cur.fetchone())
conn.close()
```

Çalıştırma (PowerShell):

```powershell
python main.py
```

---

Yardım gerekir veya tablo/sütun adları farklı görünüyorsa bana bildirin; ben veritabanı şemasını kontrol edip sorguları sizin için uyarlayabilirim.

İyi çalışmalar!
