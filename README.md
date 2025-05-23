📘 Capi Kişi Sorgulama API Eğitimi – Rehber

Bu doküman, sıfırdan API geliştirmenin temelini öğretmek amacıyla hazırlanmıştır. Proje bir hizmet olarak sunulmaz, eğitim ve öğrenim amaçlı hazırlanmıştır. Kendi API’nizi geliştirmeniz ve özelleştirmeniz için bir başlangıç noktası sağlar.

> ⚠️ UYARI: Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.




---

🎯 1. Projenin Amacı

Bu projeyle, aşağıdaki kazanımları elde edeceksiniz:

Flask ile RESTful API oluşturma mantığını öğrenirsiniz.

JSON, YAML veya internet üzerinden veri yüklemeyi öğrenirsiniz.

Rate limit (istek sınırı), logging (günlükleme) gibi profesyonel özellikleri entegre etmeyi öğrenirsiniz.

Kendi API’nizi geliştirip çalıştırabilecek yapıyı kurmuş olursunuz.



---

📁 2. Dosya ve Klasör Yapısı

CapiKisiAPI/                 # Proje ana dizini
├── app.py                   # Ana Flask API uygulaması
├── config.json              # Ayar dosyası (port, limit, kaynak)
├── requirements.txt         # Gerekli kütüphaneler
├── data/                    # Veri kaynakları (örnek JSON/YAML)
│   ├── kisiler.json
│   └── kisiler.yaml
├── logs/                    # API günlükleri (log)
│   └── app.log
└── README.txt               # Bu eğitim dökümanı


---

🧠 2.1. Klasör Dosyaları (Kodlarla Birlikte)

📄 app.py

from flask import Flask, jsonify, request
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import logging
import json
import yaml
import requests

with open("config.json", "r", encoding="utf-8") as f:
    config = json.load(f)

data_source = config["data_source"]
app = Flask(__name__)
limiter = Limiter(get_remote_address, app=app, default_limits=[config["rate_limit"]])

logging.basicConfig(filename="logs/app.log", level=getattr(logging, config["logging_level"].upper()),
                    format='%(asctime)s - %(message)s')

def veri_yukle():
    try:
        if data_source["type"] == "file":
            if data_source["path"].endswith(".json"):
                with open(data_source["path"], "r", encoding="utf-8") as f:
                    return json.load(f)
            elif data_source["path"].endswith(".yaml"):
                with open(data_source["path"], "r", encoding="utf-8") as f:
                    return yaml.safe_load(f)
        elif data_source["type"] == "url":
            r = requests.get(data_source["path"])
            return r.json()
    except:
        return []

def kisi_ara(tip, deger):
    veri = veri_yukle()
    return [kisi for kisi in veri if kisi.get(tip, '').lower() == deger.lower()]

@app.route("/capiapi/<tip>/<deger>")
@limiter.limit(config["rate_limit"])
def sorgula(tip, deger):
    logging.info(f"/capiapi/{tip}/{deger} - IP: {request.remote_addr}")
    sonuc = kisi_ara(tip, deger)
    if sonuc:
        return jsonify({"durum": "başarılı", "veri": sonuc}), 200
    return jsonify({"durum": "hata", "mesaj": "Kayıt bulunamadı.", "ip": request.remote_addr, "sorgu": deger}), 404

@app.errorhandler(404)
def hata404(e):
    return jsonify({"durum": "hata", "mesaj": "Geçersiz istek adresi."}), 404

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=config["port"], ssl_context="adhoc")

📄 config.json

{
  "port": 443,
  "rate_limit": "10/minute",
  "logging_level": "INFO",
  "data_source": {
    "type": "file",
    "path": "data/kisiler.json"
  }
}

📄 requirements.txt

flask
flask-limiter
pyyaml
requests

📄 data/kisiler.json

[
  {"ad": "Ayşe", "soyad": "Demir", "il": "Ankara", "tc": "11111111110", "gsm": "5431234567"},
  {"ad": "Murat", "soyad": "Kaya", "il": "İstanbul", "tc": "11111111111", "gsm": "5437654321"}
]

📄 data/kisiler.yaml

- ad: Ayşe
  soyad: Demir
  il: Ankara
  tc: "11111111110"
  gsm: "5431234567"
- ad: Murat
  soyad: Kaya
  il: İstanbul
  tc: "11111111111"
  gsm: "5437654321"

📄 logs/app.log

> Bu dosya, uygulama çalıştırıldığında otomatik oluşturulur ve günlük kayıtlarını içerir.


---

⚙️ 3. Kurulum ve Ortam Hazırlığı

Bu proje çalıştırılmak için değil, öğrenilmek içindir. Yine de çalıştırmak isteyenler için örnek kurulum süreci:

1. Python ve pip yüklü olmalı

2. Sanal ortam kurulumu (opsiyonel ama önerilir)

python -m venv venv
# Linux/macOS: source venv/bin/activate
# Windows:     venv\Scripts\activate

3. Kütüphanelerin yüklenmesi

pip install -r requirements.txt

4. Örnek yapılandırma dosyası (config.json)

Yukarıda belirtilmiştir.


---

🔌 4. API Nasıl Çalışır? Kodun Yapısı Nedir?

app.py dosyasındaki kod, aşağıdaki görevleri yerine getirir:

config.json içinden ayarları okur (port, rate limit, veri kaynağı).

Flask uygulamasını başlatır.

Verileri .json, .yaml dosyasından ya da bir URL’den okur.

Gelen endpoint isteklerine göre filtreleme yapar ve sonuç döner.

Loglama ile istekleri terminale veya log dosyasına yazar.


Kodun içinde yorum satırları ile açıklamalar yer alır. Her endpoint bir sorgu türünü temsil eder.


---

🔍 5. API Sorgu Örnekleri ve Yapısı

Her sorgu HTTPS ile çalışır. Adres yapısı aşağıdaki gibidir:

https://örnekapı.net/capiapi/<endpoint>/<değer>

Sorgu Örnekleri:

.../capiapi/adsoyadsorgu/ayse

.../capiapi/adsoyadilsorgu/ayseankara

.../capiapi/adressorgu/11111111110

.../capiapi/gsmtcsorgu/5431234567

.../capiapi/tcgsmsorgu/11111111110

.../capiapi/ailesorgu/11111111110

.../capiapi/sülalesorgu/11111111110


Başarılı JSON Yanıt Örneği:

{
  "durum": "başarılı",
  "veri": [ { "ad": "Ayşe", "soyad": "Demir", "il": "Ankara" } ]
}

Hatalı JSON Yanıt Örneği:

{
  "durum": "hata",
  "mesaj": "Kayıt bulunamadı.",
  "sorgu": "ayse",
  "ip": "192.168.1.5"
}

Rate Limit Aşımı:

{
  "durum": "hata",
  "mesaj": "Çok fazla istek. Lütfen biraz bekleyin."
}


---

💡 6. Geliştirme Fikirleri – Öğrenmeye Devam!

API’yi daha profesyonel hale getirmek için eklemeniz önerilir:

🔑 JWT token ile erişim doğrulama

📊 Swagger ile canlı dökümantasyon

🔄 Redis ile önbellekleme (caching)

🧩 SQLAlchemy ile veritabanı desteği

📈 Prometheus & Grafana ile izleme

📥 Panel üzerinden veri ekleme/silme



---

👨‍🏫 7. Sonuç ve Öğrenme Hedefi

Bu proje:

Öğretme ve öğrenme temellidir.

Çalışan bir sistem değil, örnek yapı ve kod öğreticisidir.

Kendi API’nizi geliştirebilmeniz için rehberdir.


İsterseniz kodları değiştirin, farklı veri kaynaklarıyla test edin ve profesyonel bir API geliştiricisi olma yolunda ilk adımı atın!


---

📄 Lisans

MIT Lisansı © 2025 Capi


---

👋 Sorularınız İçin

Telegram: @capiyedek

GitHub: github.com/byblackcapi


> Mutlu kodlamalar! 🚀



