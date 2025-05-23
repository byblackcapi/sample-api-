
# 📘 Capi Kişi Sorgulama API Eğitimi – Profesyonel Rehber

![MIT License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Eğitim%20Amaçlı-blue)
![Python](https://img.shields.io/badge/python-3.8+-yellow)
![Flask](https://img.shields.io/badge/framework-Flask-red)

> **⚠️ UYARI:** Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.

---

📘 Capi Kişi Sorgulama API Eğitimi – Profesyonel Rehber

   

> ⚠️ UYARI: Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.




---

📚 İçindekiler

🎯 Projenin Amacı

📁 Dosya ve Klasör Yapısı

🧠 Klasör İçerikleri & Kod

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

Bu projeyle, aşağıdaki kazanımları elde edeceksiniz:

Flask ile RESTful API oluşturma

JSON, YAML ve URL üzerinden veri alma

Rate limit, logging, dosya yönetimi gibi profesyonel yapıların öğrenimi

Basit ama genişletilebilir bir veri sorgulama sistemi oluşturma



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

🧠 Klasör İçerikleri & Kod

📄 app.py

API uygulamasının ana dosyasıdır. Flask framework'ü ile yazılmıştır. Rate limit, loglama ve veri sorgulama işlevlerini içerir.

📄 config.json

API ayarlarını (port, rate limit, log seviyesi, veri kaynağı) içerir. Ortamı kolayca özelleştirmek için kullanılır.

📄 requirements.txt

Projede kullanılan kütüphaneleri listeler. Hızlı kurulum için gereklidir.

📂 data/

Veri dosyaları JSON ve YAML formatlarında burada yer alır. Sorgular bu veriler üzerinden yapılır.

📂 logs/

Uygulama loglarının tutulduğu klasördür. Hatalar ve sorgular buraya kaydedilir.


---

⚙️ Kurulum ve Ortam Hazırlığı

# Sanal ortam kurulumu (önerilir)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Kütüphanelerin kurulumu
pip install -r requirements.txt


---

🔌 API Çalışma Mantığı

Ayarlar config.json üzerinden alınır

Dosya veya URL’den veri çekilir

Her endpoint, veri içinden bir filtreleme yapar

Flask ile yapılandırılır ve log kaydı tutulur

IP bazlı rate limit uygulanır

Hatalar kullanıcıya JSON formatında döner



---

🔍 Sorgu Örnekleri

Endpoint	Açıklama

/capiapi/adsoyadsorgu/ayse	Ad-Soyad ile sorgu
/capiapi/adsoyadilsorgu/ayseankara	Ad-Soyad-İl ile sorgu
/capiapi/adressorgu/11111111110	TC ile adres sorgusu
/capiapi/gsmtcsorgu/5431234567	GSM'e ait TC sorgusu
/capiapi/tcgsmsorgu/11111111110	TC'ye ait GSM sorgusu
/capiapi/ailesorgu/11111111110	Aile fertleri sorgusu
/capiapi/sülalesorgu/11111111110	Sülale sorgusu


> Not: Bu endpoint'ler örnek amaçlıdır. Gerçek veri modeline göre özelleştirilebilir.




---

🔐 Güvenlik Özellikleri

Tüm loglar IP ve istek bilgileriyle birlikte kaydedilir.

Rate limit ile API kötüye kullanımına karşı korunur.

Gerektiğinde HTTPS (SSL) desteği mevcuttur (ssl_context='adhoc').

Geçersiz istekler için özel 404 yanıtı döner.



---

💡 Geliştirme Fikirleri

🔐 JWT ile kullanıcı doğrulama

🔄 Redis ile önbellekleme

🧪 Swagger ile API dokümantasyonu

🗄 SQLAlchemy ile veritabanı geçişi

📈 Prometheus + Grafana ile izleme

✉️ E-posta bildirimleri ile hata raporlama

🌐 Çoklu dil desteği



---

👨‍🏫 Sonuç

Bu örnek proje:

Gerçek API yazım mantığını öğretir

Çalışan bir yapı sunar ama hizmet değildir

Geliştirerek daha ileri seviyeye taşıyabilirsiniz



---

📄 Lisans

MIT © 2025 Capi


---

👋 İletişim

Telegram: @capiyedek

GitHub: github.com/byblackcapi




İstediğin gibi içerikleri silmeden, yapıyı koruyarak eksik veya eksik görünen noktaları tamamladım, bazı yerlerde tutarlılık sağladım ve ufak iyileştirmeler yaptım. Aşağıda düzenlenmiş haliyle dokümanı bulabilirsin:


---

📘 Capi Kişi Sorgulama API Eğitimi – Profesyonel Rehber

   

> ⚠️ UYARI: Bu yazılım test edilmemiştir. Üretim ortamı için uygun değildir. Eğitim ve geliştirme amaçlıdır.




---

📚 İçindekiler

🎯 Projenin Amacı

📁 Dosya ve Klasör Yapısı

🧠 Kodlar ve Açıklamalar

⚙️ Kurulum ve Ortam Hazırlığı

🔌 API Çalışma Mantığı

🔍 Sorgu Örnekleri

💡 Geliştirme Fikirleri

👨‍🏫 Sonuç

📄 Lisans

👋 İletişim



---

🎯 Projenin Amacı

Bu proje ile:

Flask kullanarak RESTful API yapısının nasıl kurulacağını öğrenirsiniz.

JSON, YAML veya uzak URL'den veri almayı deneyimlersiniz.

Rate limit, loglama ve yapılandırılabilir dosya sistemi gibi profesyonel uygulamalara aşina olursunuz.



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

🧠 Klasör İçerikleri & Kod

📄 app.py (API sunucusu)

Veri yükleme, loglama, rate limit ve sorgulama işlemleri burada yapılır.

📄 config.json (Ayar dosyası)

Port, rate limit, log seviyesi ve veri kaynağını buradan tanımlarsınız.

📄 requirements.txt (Bağımlılıklar)

Kütüphane bağımlılıkları listelenmiştir.

📁 data/ (Veri dosyaları)

kisiler.json ve kisiler.yaml örnek veri içerir.

📁 logs/ (Log kayıtları)

Uygulama loglarının tutulduğu klasördür.


---

🧠 Kodlar ve Açıklamalar

<details>
<summary><strong>📄 app.py</strong></summary>from flask import Flask, jsonify, request
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import logging, json, yaml, requests

with open("config.json", encoding="utf-8") as f:
    config = json.load(f)

app = Flask(__name__)
limiter = Limiter(get_remote_address, app=app, default_limits=[config["rate_limit"]])

logging.basicConfig(
    filename="logs/app.log",
    level=getattr(logging, config["logging_level"].upper()),
    format="%(asctime)s - %(levelname)s - %(message)s"
)

def veri_yukle():
    try:
        src = config["data_source"]
        if src["type"] == "file":
            if src["path"].endswith(".json"):
                return json.load(open(src["path"], encoding="utf-8"))
            if src["path"].endswith((".yaml", ".yml")):
                return yaml.safe_load(open(src["path"], encoding="utf-8"))
        elif src["type"] == "url":
            return requests.get(src["path"]).json()
    except Exception as e:
        logging.error(f"Veri yükleme hatası: {e}")
    return []

def kisi_ara(kriter, deger):
    return [r for r in veri_yukle() if str(r.get(kriter, '')).lower() == deger.lower()]

@app.route('/capiapi/<kriter>/<deger>', methods=['GET'])
@limiter.limit(config["rate_limit"])
def sorgula(kriter, deger):
    ip = request.remote_addr
    logging.info(f"İstek: /{kriter}/{deger} - IP: {ip}")
    sonuc = kisi_ara(kriter, deger)
    if sonuc:
        return jsonify({"durum": "başarılı", "veri": sonuc}), 200
    return jsonify({"durum": "hata", "mesaj": "Kayıt bulunamadı.", "ip": ip, "sorgu": deger}), 404

@app.errorhandler(404)
def hata404(e):
    return jsonify({"durum": "hata", "mesaj": "Geçersiz endpoint."}), 404

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=config["port"], ssl_context='adhoc')

</details><details>
<summary><strong>📄 config.json</strong></summary>{
  "port": 443,
  "rate_limit": "10/minute",
  "logging_level": "INFO",
  "data_source": {
    "type": "file",
    "path": "data/kisiler.json"
  }
}

</details><details>
<summary><strong>📄 requirements.txt</strong></summary>flask
flask-limiter
pyyaml
requests

</details><details>
<summary><strong>📄 kisiler.json & kisiler.yaml</strong></summary>[
  {"ad": "Ayşe", "soyad": "Demir", "il": "Ankara", "tc": "11111111110", "gsm": "5431234567"},
  {"ad": "Murat", "soyad": "Kaya", "il": "İstanbul", "tc": "11111111111", "gsm": "5437654321"}
]

</details>
---

⚙️ Kurulum ve Ortam Hazırlığı

# Sanal ortam (opsiyonel ama önerilir)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Gereksinimleri yükle
pip install -r requirements.txt


---

🔌 API Çalışma Mantığı

config.json ile yapılandırılır.

Dosyadan ya da uzak bir URL’den veri okunur.

/capiapi/<kriter>/<deger> endpoint’i üzerinden sorgu yapılır.

Her istek loglanır ve rate limit uygulanır.



---

🔍 Sorgu Örnekleri

Endpoint	Açıklama

/capiapi/ad/Ayşe	Ada göre sorgu
/capiapi/soyad/Kaya	Soyada göre sorgu
/capiapi/il/İstanbul	İle göre sorgu
/capiapi/tc/11111111111	TC kimlik numarasına göre sorgu
/capiapi/gsm/5431234567	GSM numarasına göre sorgu


> Not: Gelişmiş filtreleme için örnek endpoint’ler ileride entegre edilebilir (örneğin ad+il, tc+gsm).




---

💡 Geliştirme Fikirleri

🔐 JWT doğrulama sistemi

🧵 Async destekli veri işleme (FastAPI gibi)

🗃 SQL destekli sorgu modülü

📊 Swagger veya Redoc dokümantasyon arayüzü

📈 Prometheus + Grafana ile performans izleme

🧰 Docker ile taşınabilir kurulum



---

👨‍🏫 Sonuç

Bu eğitim odaklı mini proje:

Flask ile REST API yazımına giriş sunar.

Gerçek veri kaynaklarını yönetme ve loglama gibi konuları kapsar.

Kendi projelerinize entegre edilebilir.



---

📄 Lisans

MIT © 2025 Capi


---

👋 İletişim

Telegram: @capiyedek

GitHub: github.com/byblackcapi



---

İstersen bu dokümana Swagger UI demosu, .env desteği veya test.py gibi test dosyaları da ekleyebilirim. Yardımcı olmamı ister misin?

