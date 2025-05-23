🚀 Capi Kişi Sorgulama API Eğitimi – Profesör Rehberi 📘

Bu doküman, adım adım RESTful API geliştirmeyi öğretmek amacıyla hazırlanmıştır. Üretim için değil, eğitim ve öğrenim amaçlı bir yapı sunar. Kendi API’nizi özelleştirerek geliştirebilmeniz için gerekli tüm bilgiler aşağıdadır.

⚠️ UYARI: Bu yazılım test edilmemiştir. Gerçek projelerde kullanmadan önce mutlaka kapsamlı test yapın.


---

🎯 1. Projenin Amacı

🛠️ Flask tabanlı bir REST API kurulumunu öğrenmek

📂 JSON, YAML veya uzak URL’den veri yüklemeyi kavramak

⏱️ Rate limit (istek sınırı) ve logging (günlükleme) entegrasyonunu görmek

🚀 Kendi API’nizi baştan sona şablon şeklinde oluşturmak



---

📁 2. Klasör & Dosya Yapısı

CapiKisiAPI/               # Proje kök dizini
├── 📄 app.py               # Flask API uygulaması
├── ⚙️ config.json         # Ayar dosyası (port, limit, kaynak)
├── 📦 requirements.txt    # Python bağımlılıkları
├── 🗄️ data/               # Test veri kaynakları
│   ├── 🌐 kisiler.json     # Örnek JSON verisi
│   └── 📝 kisiler.yaml     # Örnek YAML verisi
├── 📑 logs/               # Log dosyaları
│   └── 🗒️ app.log         # Uygulama günlükleri
└── 📘 README.txt          # Bu eğitim dokümanı


---

🧠 2.1. Klasör İçerikleri & Kod Örnekleri

📄 app.py

from flask import Flask, jsonify, request
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import logging, json, yaml, requests

# — Konfigürasyon —
with open("config.json", encoding="utf-8") as f:
    config = json.load(f)

data_source = config["data_source"]

app = Flask(__name__)
limiter = Limiter(get_remote_address, app=app, default_limits=[config["rate_limit"]])

# — Logging Ayarları —
logging.basicConfig(
    filename="logs/app.log",
    level=getattr(logging, config["logging_level"].upper()),
    format="%(asctime)s - %(levelname)s - %(message)s"
)

# — Veri Yükleme Fonksiyonu —
def veri_yukle():
    try:
        t = data_source["type"]
        p = data_source["path"]
        if t == "file":
            if p.endswith('.json'):
                return json.load(open(p, encoding='utf-8'))
            if p.endswith(('.yaml', '.yml')):
                return yaml.safe_load(open(p, encoding='utf-8'))
        elif t == "url":
            return requests.get(p).json()
    except Exception as e:
        logging.error(f"Veri yüklenirken hata: {e}")
    return []

# — Genel Sorgulama Fonksiyonu —
def kisi_ara(kriter, deger):
    data = veri_yukle()
    return [item for item in data if str(item.get(kriter, '')).lower() == deger.lower()]

# — Dinamik Endpoint —
@app.route('/capiapi/<string:kriter>/<string:deger>', methods=['GET'])
@limiter.limit(config["rate_limit"])
def sorgula(kriter, deger):
    ip = request.remote_addr
    logging.info(f"İstek: /capiapi/{kriter}/{deger} - IP: {ip}")
    sonuc = kisi_ara(kriter, deger)
    if sonuc:
        return jsonify({"durum": "başarılı", "veri": sonuc}), 200
    return jsonify({
        "durum": "hata",
        "mesaj": "Kayıt bulunamadı.",
        "sorgu": deger,
        "ip": ip
    }), 404

# — Hata Yönetimi —
@app.errorhandler(404)
def not_found(e):
    return jsonify({"durum": "hata", "mesaj": "Geçersiz endpoint."}), 404

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=config['port'], ssl_context='adhoc')


---

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


---

📄 requirements.txt

flask
flask-limiter
pyyaml
requests


---

📄 data/kisiler.json

[
  {"ad":"Ayşe","soyad":"Demir","il":"Ankara","tc":"11111111110","gsm":"5431234567"},
  {"ad":"Murat","soyad":"Kaya","il":"İstanbul","tc":"11111111111","gsm":"5437654321"}
]


---

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


---

📄 logs/app.log

> Uygulama çalıştıkça oluşturulur ve tarih, seviye ve mesaj içerir.




---

⚙️ 3. Kurulum & Ortam Hazırlığı

Bu yapı çalıştırma değil, öğrenme içindir. Yine de denemek isteyenler:

# 1. Sanal ortam
python -m venv venv
# Linux/macOS
source venv/bin/activate
# Windows
venv\\Scripts\\activate

# 2. Bağımlılıklar
pip install -r requirements.txt

# 3. config.json düzenleme (dosya yolunuzu kontrol edin)
# 4. Log klasörü oluşturun: mkdir logs
# 5. Uygulamayı başlatın:
python app.py


---

🔍 4. API Sorgu Örnekleri

Tüm istekler HTTPS üzerinden:

https://örnekapı.net/capiapi/<kriter>/<deger>

🆔 adsoyadsorgu/ayse

📍 adsoyadilsorgu/ayseankara

🏠 adressorgu/11111111110

📱 gsmtcsorgu/5431234567

🔢 tcgsmsorgu/11111111110

🏡 ailesorgu/11111111110

👪 sülalesorgu/11111111110


Başarılı Cevap:

{ "durum":"başarılı", "veri":[{"ad":"Ayşe","soyad":"Demir","il":"Ankara"}] }

Hata Cevabı:

{ "durum":"hata","mesaj":"Kayıt bulunamadı.","sorgu":"ayse","ip":"127.0.0.1" }

Harika! Artık kendi API’sini profesyonelce nasıl kurup çalıştıracağını biliyorsun. 👩‍🏫👨‍🏫


---

🌟 5. İleri Seviye Geliştirme Önerileri

🔑 JWT ile kimlik doğrulama

📝 Swagger/OpenAPI dokümantasyonu

⚡ Caching (Redis) entegrasyonu

🗄️ Veritabanı (SQLAlchemy) bağlantısı

📈 Monitoring (Prometheus+Grafana)



---

🤝 Katkı & İletişim

Fork & Pull Request 🎉

Telegram: @capiyedek

GitHub: github.com/byblackcapi



---

MIT Lisansı © 2025 Capi

