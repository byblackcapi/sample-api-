
# 📘 Capi Kişi Sorgulama API Eğitimi – Rehber

![MIT License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Eğitim%20Amaçlı-blue)
![Python](https://img.shields.io/badge/python-3.8+-yellow)
![Flask](https://img.shields.io/badge/framework-Flask-red)
![RESTful](https://img.shields.io/badge/API-RESTful-lightgrey)
![Rate Limit](https://img.shields.io/badge/rate--limit-enabled-brightgreen)
![Logging](https://img.shields.io/badge/logging-active-orange)
![YAML Support](https://img.shields.io/badge/YAML-supported-blueviolet)
![JSON Support](https://img.shields.io/badge/JSON-supported-lightblue)
![SSL](https://img.shields.io/badge/SSL-AdHoc-informational)
![Deployment](https://img.shields.io/badge/deploy-localhost-red)
![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Last Update](https://img.shields.io/badge/last%20update-May%202025-critical)
![Contributions](https://img.shields.io/badge/contributions-welcome-success)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20MacOS-black)
> **⚠️ UYARI:** Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.

---

📚 **İçindekiler**

- 🎯 Projenin Amacı  
- 📁 Dosya ve Klasör Yapısı  
- 🧠 Kodlar ve Açıklamalar  
- ⚙️ Kurulum ve Ortam Hazırlığı  
- 🔌 API Çalışma Mantığı  
- 📊 Veri Formatı ve Örnekler  
- 🔍 Sorgu Örnekleri  
- ✅ Başarı ve Hata Yanıtları  
- 🔐 Güvenlik Özellikleri  
- 💡 Geliştirme Fikirleri  
- 👨‍🏫 Sonuç  
- 📄 Lisans  
- 👋 İletişim  

---

🎯 **Projenin Amacı**

Bu proje ile:
- Flask kullanarak RESTful API yapısının nasıl kurulacağını öğrenirsiniz.
- JSON, YAML veya uzak URL'den veri almayı deneyimlersiniz.
- Rate limit, loglama ve yapılandırılabilir dosya sistemi gibi profesyonel uygulamalara aşina olursunuz.

---

📁 **Dosya ve Klasör Yapısı**

```
CapiKisiAPI/
├── app.py
├── config.json
├── requirements.txt
├── data/
│   ├── kisiler.json
│   └── kisiler.yaml
├── logs/
│   └── app.log
└── README.txt
```

---

🧠 **Kodlar ve Açıklamalar**

**app.py**
```python
from flask import Flask, jsonify, request
import json, yaml, logging
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

app = Flask(__name__)
limiter = Limiter(get_remote_address, app=app, default_limits=["10 per minute"])

with open("config.json") as f:
    config = json.load(f)

logging.basicConfig(filename="logs/app.log", level=logging.INFO)

def load_data():
    if config["veri_kaynagi"].endswith(".json"):
        with open(config["veri_kaynagi"], "r", encoding="utf-8") as f:
            return json.load(f)
    elif config["veri_kaynagi"].endswith(".yaml"):
        with open(config["veri_kaynagi"], "r", encoding="utf-8") as f:
            return yaml.safe_load(f)
    return []

@app.route("/capiapi/<kriter>/<deger>")
@limiter.limit("5 per minute")
def kisi_sorgula(kriter, deger):
    veriler = load_data()
    sonuc = [k for k in veriler if k.get(kriter, "").lower() == deger.lower()]
    logging.info(f"{request.remote_addr} - {kriter}:{deger} - {len(sonuc)} sonuç")
    if not sonuc:
        return jsonify({"basari": False, "mesaj": "Kayıt bulunamadı"}), 404
    return jsonify({"basari": True, "veriler": sonuc})

@app.errorhandler(404)
def not_found(e):
    return jsonify({"hata": "Geçersiz endpoint veya parametre"}), 404

if __name__ == "__main__":
    app.run(port=config["port"], ssl_context="adhoc")
```

**config.json**
```json
{
  "port": 5000,
  "veri_kaynagi": "data/kisiler.json"
}
```

**requirements.txt**
```
Flask
flask-limiter
PyYAML
```

---

📊 **Veri Formatı ve Örnekler**

**kisiler.json**
```json
[
  {
    "ad": "Ayşe",
    "soyad": "Kaya",
    "tc": "11111111111",
    "gsm": "5431234567",
    "il": "İstanbul",
    "anne_adi": "Fatma",
    "baba_adi": "Mehmet"
  },
  {
    "ad": "Ahmet",
    "soyad": "Demir",
    "tc": "22222222222",
    "gsm": "5329876543",
    "il": "Ankara",
    "anne_adi": "Zeynep",
    "baba_adi": "Ali"
  }
]
```

---

🔍 **Sorgu Örnekleri**

| Endpoint                          | Açıklama                   |
|----------------------------------|----------------------------|
| /capiapi/ad/Ayşe                 | Ada göre sorgu             |
| /capiapi/soyad/Kaya              | Soyada göre sorgu          |
| /capiapi/il/İstanbul             | İl'e göre sorgu            |
| /capiapi/tc/11111111111          | TC'ye göre sorgu           |
| /capiapi/gsm/5431234567          | GSM'e göre sorgu           |

> Gelişmiş filtreleme endpoint'leri ileride eklenebilir.

---

✅ **Başarı ve Hata Yanıtları**

**Başarılı Yanıt:**
```json
{
  "basari": true,
  "veriler": [
    {
      "ad": "Ayşe",
      "soyad": "Kaya",
      "tc": "11111111111",
      "gsm": "5431234567",
      "il": "İstanbul",
      "anne_adi": "Fatma",
      "baba_adi": "Mehmet"
    }
  ]
}
```

**Hatalı Yanıt (Kayıt Bulunamadı):**
```json
{
  "basari": false,
  "mesaj": "Kayıt bulunamadı"
}
```

**Geçersiz Endpoint:**
```json
{
  "hata": "Geçersiz endpoint veya parametre"
}
```

---

🔐 **Güvenlik Özellikleri**

- IP & istek loglama  
- Rate limit ile koruma  
- SSL desteği (`ssl_context='adhoc'`)  
- Geçersiz istekler için özel 404

---

💡 **Geliştirme Fikirleri**

- JWT ile kimlik doğrulama  
- Redis ile önbellekleme  
- Swagger/OpenAPI dokümantasyonu  
- SQLAlchemy entegrasyonu  
- Prometheus/Grafana ile izleme  
- E-posta ile hata bildirim sistemi  
- Çoklu dil desteği

---

👨‍🏫 **Sonuç**

Bu eğitim projesi:
- Flask ile REST API yazımına giriş sağlar
- Loglama ve rate limit gibi profesyonel teknikleri gösterir
- Kolayca genişletilebilir yapıdadır

---

📄 **Lisans**

MIT © 2025 Capi

---

👋 **İletişim**

- Telegram: @capiyedek  
- GitHub: [github.com/byblackcapi](https://github.com/byblackcapi)
