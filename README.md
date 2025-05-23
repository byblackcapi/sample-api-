<!-- README.md for GitHub -->📘 Capi Kişi Sorgulama API Eğitimi – Profesör Rehberi

    

> Bu doküman, sıfırdan API geliştirmenin temellerini öğretmek amacıyla hazırlanmıştır. Üretim için uygun değildir; eğitim ve öğrenim amaçlıdır.



⚠️ UYARI: Kapsamlı testler yapılmadan gerçek verilerle kullanmayın.


---

📋 İçindekiler

🎯 Projenin Amacı

📁 Dosya ve Klasör Yapısı

🧠 Klasör İçerikleri & Kod

app.py

config.json

requirements.txt

data/


⚙️ Kurulum & Ortam Hazırlığı

🔍 API Sorgu Örnekleri

🌟 İleri Seviye Öneriler

🤝 Katkıda Bulunma

📄 Lisans



---

🎯 Projenin Amacı

Flask ile RESTful API oluşturma.

JSON, YAML veya URL üzerinden veri yükleme.

Rate limit ve logging entegrasyonu.

Öğrenme amaçlı şablon sunumu.



---

📁 Dosya ve Klasör Yapısı

CapiKisiAPI/              # Proje kök dizini
├── app.py                # 🖥️ Flask API uygulaması
├── config.json           # ⚙️ Ayarlar: port, limit, kaynak
├── requirements.txt      # 📦 Python bağımlılıkları
├── data/                 # 🗄️ Örnek veri kaynakları
│   ├── kisiler.json
│   └── kisiler.yaml
├── logs/                 # 📑 API günlükleri
│   └── app.log
└── README.md             # 📘 Bu doküman


---

🧠 Klasör İçerikleri & Kod

<details>
<summary>📄 <code>app.py</code> (tıkla aç)</summary>from flask import Flask, jsonify, request
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import logging, json, yaml, requests

# — Konfigürasyon —
with open("config.json", encoding="utf-8") as f:
    config = json.load(f)

app = Flask(__name__)
limiter = Limiter(get_remote_address, app=app, default_limits=[config["rate_limit"]])

logging.basicConfig(
    filename="logs/app.log",
    level=getattr(logging, config["logging_level"].upper()),
    format="%(asctime)s - %(levelname)s - %(message)s"
)

data_source = config["data_source"]

# — Veri Yükleme —
def veri_yukle():
    try:
        t, p = data_source["type"], data_source["path"]
        if t == "file":
            if p.endswith('.json'):
                return json.load(open(p, encoding='utf-8'))
            if p.endswith(('.yaml', '.yml')):
                return yaml.safe_load(open(p, encoding='utf-8'))
        elif t == "url":
            return requests.get(p).json()
    except Exception as e:
        logging.error(f"Veri yüklemede hata: {e}")
    return []

# — Sorgulama —
def kisi_ara(kriter, deger):
    return [r for r in veri_yukle() if str(r.get(kriter, '')).lower() == deger.lower()]

@app.route('/capiapi/<kriter>/<deger>', methods=['GET'])
@limiter.limit(config["rate_limit"])
def sorgula(kriter, deger):
    ip = request.remote_addr
    logging.info(f"İstek: {kriter}/{deger} - IP: {ip}")
    sonuc = kisi_ara(kriter, deger)
    if sonuc:
        return jsonify({"durum": "başarılı", "veri": sonuc}), 200
    return jsonify({
        "durum": "hata", "mesaj": "Kayıt bulunamadı.",
        "sorgu": deger, "ip": ip
    }), 404

@app.errorhandler(404)
def not_found(e):
    return jsonify({"durum": "hata", "mesaj": "Geçersiz endpoint."}), 404

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=config['port'], ssl_context='adhoc')

</details><details>
<summary>📄 <code>config.json</code></summary>{
  "port": 443,
  "rate_limit": "10/minute",
  "logging_level": "INFO",
  "data_source": { "type": "file", "path": "data/kisiler.json" }
}

</details><details>
<summary>📄 <code>requirements.txt</code></summary>flask
flask-limiter
pyyaml
requests

</details><details>
<summary>📄 <code>data/kisiler.json</code></summary>[
  {"ad":"Ayşe","soyad":"Demir","il":"Ankara","tc":"11111111110","gsm":"5431234567"},
  {"ad":"Murat","soyad":"Kaya","il":"İstanbul","tc":"11111111111","gsm":"5437654321"}
]

</details><details>
<summary>📄 <code>data/kisiler.yaml</code></summary>- ad: Ayşe
  soyad: Demir
  il: Ankara
  tc: "11111111110"
  gsm: "5431234567"
- ad: Murat
  soyad: Kaya
  il: İstanbul
  tc: "11111111111"
  gsm: "5437654321"

</details><details>
<summary>📄 <code>logs/app.log</code></summary>> Uygulama çalıştırıldığında otomatik oluşturulur ve günlük kayıtlarını içerir.



</details>
---

⚙️ Kurulum & Ortam Hazırlığı

Bu proje öğrenme amaçlıdır ancak denemek isterseniz:

# 1. Sanal ortam
python -m venv venv
# Linux/macOS	source venv/bin/activate
# Windows	venv\\Scripts\\activate

# 2. Bağımlılıkları yükle
pip install -r requirements.txt

# 3. config.json'u düzenleyin
# 4. Log klasörünü oluşturun: mkdir logs
# 5. Uygulamayı çalıştırın:
python app.py


---

🔍 API Sorgu Örnekleri

HTTPS kullanarak:

https://örnekapı.net/capiapi/<kriter>/<deger>

Kriter	Açıklama	Örnek

adsoyadsorgu	İsim & Soyisim	/adsoyadsorgu/ayse
adsoyadilsorgu	İsim & İl	/adsoyadilsorgu/muratistanbul
adressorgu	TC ile adres	/adressorgu/11111111110
gsmtcsorgu	GSM ile TC	/gsmtcsorgu/5431234567
tcgsmsorgu	TC ile GSM	/tcgsmsorgu/11111111110
ailesorgu	TC ile aile	/ailesorgu/11111111110
sülalesorgu	TC ile soy aile	/sülalesorgu/11111111110


Başarılı Cevap (200):

{ "durum": "başarılı", "veri": [ { "ad":"Ayşe","soyad":"Demir","il":"Ankara" } ] }

Hata (404):

{ "durum":"hata","mesaj":"Kayıt bulunamadı.","sorgu":"ayse","ip":"127.0.0.1" }

Rate limit (429):

{ "durum":"hata","mesaj":"Çok fazla istek. Lütfen sonra deneyin." }


---

🌟 İleri Seviye Öneriler

🔑 JWT ile kimlik doğrulama

📝 Swagger/OpenAPI entegrasyonu

⚡ Redis ile caching

🗄️ SQLAlchemy veritabanı desteği

📈 Prometheus & Grafana monitoring



---

🤝 Katkıda Bulunma

1. Repo'yu fork’layın 🐑


2. Yeni dal oluşturun (git checkout -b feature/...)


3. Değişiklik yapın & commit edin


4. Pull request oluşturun 🚀




---

📄 Lisans

MIT © 2025 Capi


---

👋 İletişim

Telegram: @capiyedek

GitHub: github.com/byblackcapi


> Happy coding! 🎉

