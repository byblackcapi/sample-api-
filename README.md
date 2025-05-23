# 🎉 CapiAPI – Kişi Bilgi Sorgulama REST API  

> ⚠️ **UYARI:** Bu proje **test edilmemiştir**. Gerçek verilerle kullanmadan önce lütfen kapsamlı test yapınız!

---

## 🚀 Özellikler  
- Dosya tabanlı, hafif ve hızlı kurulum  
- **Flask** + **Flask-Limiter** ile güvenli API  
- IP bazlı **rate limit** (örr: 10 istek/dakika)  
- **Configurable** port ve rate-limit ayarları  
- JSON formatında temiz hata & başarı yanıtları  
- İstekleri otomatik **log**’lama  

---

## 📂 Proje Dosya Yapısı

. ├── app.py
├── config.json
├── kisiler.json
└── README.md

---

## ⚙️ Kurulum & Başlatma

1. **Python 3** yüklü olduğundan emin olun.  
2. Gerekli paketleri yükleyin:  
   ```bash
   pip install flask flask-limiter

3. config.json dosyasını düzenleyin:

{
  "port": 8080,
  "rate_limit": "10/minute"
}


4. kisiler.json dosyasına örnek verileri ekleyin (aşağıya bakın).


5. Sunucuyu başlatın:

python app.py




---

📖 Örnek Veri – kisiler.json

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
    "soyad": "kaya",
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
  },
  {
    "ad": "Kisi6",
    "soyad": "özcan",
    "tc": "11111111115",
    "tel": "5555555550",
    "ikametgah": "edirne"
  }
]


---

🛠️ config.json Ayarları

{
  "port": 8080,
  "rate_limit": "10/minute"
}

port: API’nın dinleyeceği HTTP portu

rate_limit: IP başına dakika bazında istek limiti ("5/minute", "100/day" vb.)



---

🔗 API Endpoint

GET http://<HOST>:<PORT>/apiz/<kisi_ad>

Parametre	Açıklama

kisi_ad	Sorgulanacak kişi adı



---

🎯 Örnek Kullanım

1. Başarılı Sorgu

curl http://localhost:8080/apiz/Kisi3

Yanıt (HTTP 200):

{
  "durum": "başarılı",
  "veri": {
    "ad": "Kisi3",
    "soyad": "yılmaz",
    "tc": "11111111112",
    "tel": "5555555553",
    "ikametgah": "izmir"
  }
}

2. Kişi Bulunamadığında

curl http://localhost:8080/apiz/MevcutDegil

Yanıt (HTTP 404):

{
  "durum": "hata",
  "mesaj": "Kişi bulunamadı.",
  "ip": "127.0.0.1",
  "sorgu": "MevcutDegil"
}

3. Rate Limit Aşıldığında

Yanıt (HTTP 429):

{
  "errors": ["Rate limit exceeded: 10 per 1 minute"]
}


---

📝 Loglama

Her istek, konsola aşağıdaki formatta yazılır:

2025-05-23 14:00:00,000 - API çağrısı: /apiz/Kisi3 - IP: 192.168.1.10


---

💡 Geliştirme & İyileştirme Fikirleri

🔐 JWT veya API Key ile kimlik doğrulama

🗄️ SQLite/MongoDB gibi gerçek bir veritabanı entegrasyonu

➕ Kişi ekleme (POST), güncelleme (PUT) ve silme (DELETE) endpoint’leri

📊 Swagger/OpenAPI dokümantasyonu

🌐 Docker desteği ve CI/CD pipeline kurulumu

🔒 HTTPS ile güvenli yayın



---

📜 Lisans

Bu proje MIT Lisansı ile yayınlanmıştır.
Made with ❤️ by Capi



