
# 📘 Capi Kişi Sorgulama API Eğitimi – Profesyonel Rehber

![MIT License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Eğitim%20Amaçlı-blue)
![Python](https://img.shields.io/badge/python-3.8+-yellow)
![Flask](https://img.shields.io/badge/framework-Flask-red)

> **⚠️ UYARI:** Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.

---

## 📚 İçindekiler
- [🎯 Projenin Amacı](#-projenin-amacı)
- [📁 Dosya ve Klasör Yapısı](#-dosya-ve-klasör-yapısı)
- [🧠 Kodlar ve Açıklamalar](#-kodlar-ve-açıklamalar)
- [⚙️ Kurulum ve Ortam Hazırlığı](#️-kurulum-ve-ortam-hazırlığı)
- [🔌 API Çalışma Mantığı](#-api-çalışma-mantığı)
- [🔍 Sorgu Örnekleri](#-sorgu-örnekleri)
- [💡 Geliştirme Fikirleri](#-geliştirme-fikirleri)
- [👨‍🏫 Sonuç](#-sonuç)
- [📄 Lisans](#-lisans)
- [👋 İletişim](#-iletişim)

---

## 🎯 Projenin Amacı

Bu projeyle, aşağıdaki kazanımları elde edeceksiniz:

- Flask ile RESTful API oluşturma
- JSON, YAML ve URL üzerinden veri alma
- Rate limit, logging, dosya yönetimi gibi profesyonel yapıların öğrenimi

---

## 📁 Dosya ve Klasör Yapısı

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
└── README.md
```
## 🧠 Klasör İçerikleri & Kod
  📄 `app.py` (tıkla aç) `from flask import Flask, jsonify, request from flask_limiter import Limiter from flask_limiter.util import get_remote_address import logging, json, yaml, requests  # — Konfigürasyon — with open("config.json", encoding="utf-8") as f:     config = json.load(f)  app = Flask(__name__) limiter = Limiter(get_remote_address, app=app, default_limits=[config["rate_limit"]])  logging.basicConfig(     filename="logs/app.log",     level=getattr(logging, config["logging_level"].upper()),     format="%(asctime)s - %(levelname)s - %(message)s" )  data_source = config["data_source"]  # — Veri Yükleme — def veri_yukle():     try:         t, p = data_source["type"], data_source["path"]         if t == "file":             if p.endswith('.json'):                 return json.load(open(p, encoding='utf-8'))             if p.endswith(('.yaml', '.yml')):                 return yaml.safe_load(open(p, encoding='utf-8'))         elif t == "url":             return requests.get(p).json()     except Exception as e:         logging.error(f"Veri yüklemede hata: {e}")     return []  # — Sorgulama — def kisi_ara(kriter, deger):     return [r for r in veri_yukle() if str(r.get(kriter, '')).lower() == deger.lower()]  @app.route('/capiapi/<kriter>/<deger>', methods=['GET']) @limiter.limit(config["rate_limit"]) def sorgula(kriter, deger):     ip = request.remote_addr     logging.info(f"İstek: {kriter}/{deger} - IP: {ip}")     sonuc = kisi_ara(kriter, deger)     if sonuc:         return jsonify({"durum": "başarılı", "veri": sonuc}), 200     return jsonify({         "durum": "hata", "mesaj": "Kayıt bulunamadı.",         "sorgu": deger, "ip": ip     }), 404  @app.errorhandler(404) def not_found(e):     return jsonify({"durum": "hata", "mesaj": "Geçersiz endpoint."}), 404  if __name__ == '__main__':     app.run(host='0.0.0.0', port=config['port'], ssl_context='adhoc') `   📄 `config.json` `{   "port": 443,   "rate_limit": "10/minute",   "logging_level": "INFO",   "data_source": { "type": "file", "path": "data/kisiler.json" } } `   📄 `requirements.txt` `flask flask-limiter pyyaml requests `   📄 `data/kisiler.json` `[   {"ad":"Ayşe","soyad":"Demir","il":"Ankara","tc":"11111111110","gsm":"5431234567"},   {"ad":"Murat","soyad":"Kaya","il":"İstanbul","tc":"11111111111","gsm":"5437654321"} ] `   📄 `data/kisiler.yaml` `- ad: Ayşe   soyad: Demir   il: Ankara   tc: "11111111110"   gsm: "5431234567" - ad: Murat   soyad: Kaya   il: İstanbul   tc: "11111111111"   gsm: "5437654321" `   📄 `logs/app.log` 
---

## 🧠 Kodlar ve Açıklamalar

<details>
<summary><strong>📄 app.py</strong></summary>

```python
from flask import Flask, jsonify, request
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import logging, json, yaml, requests

with open("config.json", "r", encoding="utf-8") as f:
    config = json.load(f)

data_source = config["data_source"]
app = Flask(__name__)
limiter = Limiter(get_remote_address, app=app, default_limits=[config["rate_limit"]])

logging.basicConfig(filename="logs/app.log", level=getattr(logging, config["logging_level"].upper()), format='%(asctime)s - %(message)s')

def veri_yukle():
    try:
        if data_source["type"] == "file":
            with open(data_source["path"], "r", encoding="utf-8") as f:
                return json.load(f) if data_source["path"].endswith(".json") else yaml.safe_load(f)
        elif data_source["type"] == "url":
            return requests.get(data_source["path"]).json()
    except:
        return []

def kisi_ara(tip, deger):
    return [kisi for kisi in veri_yukle() if kisi.get(tip, '').lower() == deger.lower()]

@app.route("/capiapi/<tip>/<deger>")
@limiter.limit(config["rate_limit"])
def sorgula(tip, deger):
    logging.info(f"/capiapi/{tip}/{deger} - IP: {request.remote_addr}")
    sonuc = kisi_ara(tip, deger)
    return jsonify({"durum": "başarılı", "veri": sonuc}), 200 if sonuc else jsonify({"durum": "hata", "mesaj": "Kayıt bulunamadı.", "ip": request.remote_addr, "sorgu": deger}), 404

@app.errorhandler(404)
def hata404(e):
    return jsonify({"durum": "hata", "mesaj": "Geçersiz istek adresi."}), 404

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=config["port"], ssl_context="adhoc")
```
</details>

<details>
<summary><strong>📄 config.json</strong></summary>

```json
{
  "port": 443,
  "rate_limit": "10/minute",
  "logging_level": "INFO",
  "data_source": {
    "type": "file",
    "path": "data/kisiler.json"
  }
}
```
</details>

<details>
<summary><strong>📄 requirements.txt</strong></summary>

```
flask
flask-limiter
pyyaml
requests
```
</details>

<details>
<summary><strong>📄 kisiler.json & kisiler.yaml</strong></summary>

```json
[
  {"ad": "Ayşe", "soyad": "Demir", "il": "Ankara", "tc": "11111111110", "gsm": "5431234567"},
  {"ad": "Murat", "soyad": "Kaya", "il": "İstanbul", "tc": "11111111111", "gsm": "5437654321"}
]
```
</details>

---

## ⚙️ Kurulum ve Ortam Hazırlığı

```bash
# Sanal ortam kurulumu (önerilir)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Kütüphanelerin kurulumu
pip install -r requirements.txt
```

---

## 🔌 API Çalışma Mantığı

- Ayarlar `config.json` üzerinden alınır
- Dosya veya URL’den veri çekilir
- Her endpoint, veri içinden bir filtreleme yapar
- Flask ile yapılandırılır ve log kaydı tutulur

---

## 🔍 Sorgu Örnekleri

| Endpoint | Açıklama |
|---------|----------|
| `/capiapi/adsoyadsorgu/ayse` | Ad-Soyad ile sorgu |
| `/capiapi/adsoyadilsorgu/ayseankara` | Ad-Soyad-İl ile sorgu |
| `/capiapi/adressorgu/11111111110` | TC ile adres sorgusu |
| `/capiapi/gsmtcsorgu/5431234567` | GSM'e ait TC sorgusu |
| `/capiapi/tcgsmsorgu/11111111110` | TC'ye ait GSM sorgusu |
| `/capiapi/ailesorgu/11111111110` | Aile fertleri sorgusu |
| `/capiapi/sülalesorgu/11111111110` | Sülale sorgusu |

---

## 💡 Geliştirme Fikirleri

- 🔐 JWT ile kullanıcı doğrulama
- 🔄 Redis ile önbellekleme
- 🧪 Swagger ile API dokümantasyonu
- 🗄 SQLAlchemy ile veritabanı geçişi
- 📈 Prometheus + Grafana ile izleme

---

## 👨‍🏫 Sonuç

Bu örnek proje:
- Gerçek API yazım mantığını öğretir
- Çalışan bir yapı sunar ama hizmet değildir
- Geliştirerek daha ileri seviyeye taşıyabilirsiniz

---

## 📄 Lisans

MIT © 2025 Capi

---

## 👋 İletişim

- Telegram: [@capiyedek](https://t.me/capiyedek)
- GitHub: [github.com/byblackcapi](https://github.com/byblackcapi)
