---

# CapiAPI – Kişi Bilgi Sorgulama API’si  
**Dosya tabanlı, hafif, geliştirilebilir bir REST API sistemi**  

> ⚠️ **UYARI:** Bu proje test edilmemiştir. Gerçek verilerde kullanmadan önce detaylı test yapılmalıdır.

---

## Özellikler 🚀

- JSON dosyasına dayalı kişi sorgulama sistemi  
- Gelişmiş hata yanıtları ve IP bazlı rate limit  
- Port yapılandırması (`config.json`)  
- Basit bir REST endpoint ile sorgu  
- Her IP için dakika başına istek limiti  
- Flask ve Flask-Limiter desteği  
- Tamamen açık kaynak!

---

## Örnek Veri – `kisiler.json` 📁

```json
[
  {
    "ad": "Kisi1",
    "soyad": "kisi",
    "tc": "11111111110",
    "tel": "5555555555",
    "ikametgah": "ankara"
  },
  {
    "ad": "Kisi2",
    "soyad": "kisi",
    "tc": "11111111111",
    "tel": "5555555554",
    "ikametgah": "istanbul"
  },
  {
    "ad": "Kisi3",
    "soyad": "yılmaz",
    "tc": "11111111112",
    "tel": "5555555553",
    "ikametgah": "izmir"
  },
  {
    "ad": "Kisi4",
    "soyad": "demir",
    "tc": "11111111113",
    "tel": "5555555552",
    "ikametgah": "bursa"
  },
  {
    "ad": "Kisi5",
    "soyad": "çelik",
    "tc": "11111111114",
    "tel": "5555555551",
    "ikametgah": "antalya"
  }
]


---

Yapılandırma – config.json ⚙️

{
  "port": 8080,
  "rate_limit": "10/minute"
}

port: API'nin yayınlanacağı port

rate_limit: IP başına dakika başı sorgu limiti



---

Kurulum ve Kullanım 🔧

Gereksinimler

Python 3.x

Flask

Flask-Limiter


Kurulum

pip install flask flask-limiter

API'yi Başlat

python app.py


---

API Kullanımı 📡

Örnek İstek

GET http://localhost:8080/apiz/Kisi1

Başarılı Yanıt

{
  "durum": "başarılı",
  "veri": {
    "ad": "Kisi1",
    "soyad": "kisi",
    "tc": "11111111110",
    "tel": "5555555555",
    "ikametgah": "ankara"
  }
}

Başarısız Yanıt (Kişi Yoksa)

{
  "durum": "hata",
  "mesaj": "Kişi bulunamadı.",
  "ip": "127.0.0.1",
  "sorgu": "bilinmeyen"
}


---

Ek Özellikler 🛠

Rate Limit: Her IP için dakikada maksimum 10 sorgu

Loglama: Her API isteği konsola yazılır

Gelişmiş Hatalar: JSON formatında hata detayları



---

Geliştirme Önerileri 🧪

[ ] JWT veya API Key ile koruma

[ ] Kişi ekleme/silme/güncelleme (POST/PUT/DELETE)

[ ] SQLite veya MongoDB entegrasyonu

[ ] Admin paneli / kullanıcı girişi

[ ] Swagger/OpenAPI entegrasyonu

[ ] HTTPS destekli yayına alma



---

Lisans ve Bilgilendirme 📜

Bu proje eğitim ve test amaçlıdır. Gerçek kişisel verilerle kullanılmamalıdır!
MIT Lisansı ile sunulmuştur.

Made by Capi
GitHub: github.com/byblackcapi
Telegram Destek: t.me/capiyedek_support

---
