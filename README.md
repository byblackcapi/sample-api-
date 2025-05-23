# 📘 Capi Kişi Sorgulama API Eğitimi – Profesyonel Rehber

![MIT License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Eğitim%20Amaçlı-blue)
![Python](https://img.shields.io/badge/python-3.8+-yellow)
![Flask](https://img.shields.io/badge/framework-Flask-red)

> **⚠️ UYARI:** Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.

---

📚 İçindekiler

🎯 Projenin Amacı  
📁 Dosya ve Klasör Yapısı  
🧠 Kodlar ve Açıklamalar  
⚙️ Kurulum ve Ortam Hazırlığı  
🔌 API Çalışma Mantığı  
🔍 Sorgu Örnekleri  
🔐 Güvenlik Özellikleri  
💡 Geliştirme Fikirleri  
👨‍🏫 Sonuç  
📄 Lisans  
👋 İletişim  

---

🎯 Projenin Amacı

Bu proje ile:
- Flask kullanarak RESTful API yapısının nasıl kurulacağını öğrenirsiniz.
- JSON, YAML veya uzak URL'den veri almayı deneyimlersiniz.
- Rate limit, loglama ve yapılandırılabilir dosya sistemi gibi profesyonel uygulamalara aşina olursunuz.

---

📁 Dosya ve Klasör Yapısı

CapiKisiAPI/  
├── app.py  
├── config.json  
├── requirements.txt  
├── data/  
│   ├── kisiler.json  
│   └── kisiler.yaml  
├── logs/  
│   └── app.log  
└── README.md  

---

🧠 Kodlar ve Açıklamalar

📄 app.py – Flask API sunucusu  
📄 config.json – Ayarlar (port, rate limit vb.)  
📄 requirements.txt – Gerekli kütüphaneler  
📁 data/ – JSON/YAML veri kaynakları  
📁 logs/ – Log kayıtları

---

⚙️ Kurulum ve Ortam Hazırlığı

```bash
# Sanal ortam (önerilir)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Gerekli paketler
pip install -r requirements.txt
```

---

🔌 API Çalışma Mantığı

- Ayarlar `config.json` dosyasından alınır  
- Veri JSON, YAML veya uzak URL'den yüklenir  
- Endpoint: `/capiapi/<kriter>/<deger>`  
- IP bazlı rate limit uygulanır  
- Loglama yapılır  
- JSON formatında yanıt döner  

---

🔍 Sorgu Örnekleri

| Endpoint                          | Açıklama                   |
|----------------------------------|----------------------------|
| /capiapi/ad/Ayşe                 | Ada göre sorgu             |
| /capiapi/soyad/Kaya              | Soyada göre sorgu          |
| /capiapi/il/İstanbul             | İl'e göre sorgu            |
| /capiapi/tc/11111111111          | TC'ye göre sorgu           |
| /capiapi/gsm/5431234567          | GSM'e göre sorgu           |

> Gelişmiş filtreleme endpoint'leri ileride eklenebilir.

---

🔐 Güvenlik Özellikleri

- IP & istek loglama  
- Rate limit ile koruma  
- SSL desteği (`ssl_context='adhoc'`)  
- Geçersiz istekler için özel 404

---

💡 Geliştirme Fikirleri

- 🔐 JWT ile doğrulama  
- 🔄 Redis önbellekleme  
- 🧪 Swagger/OpenAPI dokümantasyonu  
- 🗄 SQLAlchemy entegrasyonu  
- 📈 Prometheus/Grafana ile monitoring  
- ✉️ E-posta bildirimli hata loglama  
- 🌐 Çoklu dil desteği

---

👨‍🏫 Sonuç

Bu eğitim projesi:  
- Flask ile REST API yazımına giriş sağlar  
- Loglama ve rate limit gibi profesyonel teknikleri gösterir  
- Kolayca genişletilebilir yapıdadır  

---

📄 Lisans

MIT © 2025 Capi

---

👋 İletişim

Telegram: @capiyedek  
GitHub: github.com/byblackcapi
