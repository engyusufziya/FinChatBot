# Financial AI Chatbot 📈

Bu proje, Python, PyTorch ve Hugging Face teknolojileri kullanılarak geliştirilmiş, metin tabanlı bir finansal farkındalık sohbet botudur. Kullanıcılara kendi finansal alışkanlıklarını sorgulamaları için anlamlı ve düşünmeye sevk eden sorular sorar.

Proje, **DenizBank İlerisi Gençlik Bootcamp** kapsamında geliştirilmiştir. LLM tabanlı doğrudan bir kampanya sorgulama aracı değil, genel finansal bilinç oluşturmayı hedefleyen bir uygulamadır. Bu nedenle, kampanyalara dair bilgiler **kesin/doğru olmayabilir**.

---

## İçindekiler

* [Genel Amaç](#genel-amaç)
* [Kullanılan Teknolojiler](#kullanılan-teknolojiler)
* [Kurulum](#kurulum)
* [Model Bilgisi](#model-bilgisi)
* [Sistem Çıktısı Örneği](#sistem-çıktısı-örneği)
* [Arayüz Bilgisi](#arayüz-bilgisi)
* [Gelecek Planları](#gelecek-planları)

---

## Genel Amaç

Bu uygulama, kullanıcıların kişisel finans yönetimi ve harcama alışkanlıkları üzerine düşünmelerini sağlayacak şekilde sorular üreten yapay zeka destekli bir danışman sunar.

---

## Kullanılan Teknolojiler

* Python 3.10+
* PyTorch (CUDA 12.1 destekli)
* Hugging Face Transformers
* Hugging Face Accelerate
* Hugging Face Pipeline API
* Gradio (demo arayüz)

Model olarak:

* `microsoft/Phi-4-mini-instruct` (text-generation için)

---

## Kurulum

```bash
# Google Drive'a bağlan
from google.colab import drive
drive.mount('/content/drive')

# Proje dizini oluştur
!mkdir -p /content/drive/MyDrive/hf-demo
%cd /content/drive/MyDrive/hf-demo

# Gerekli paketleri yükle
!pip install --quiet torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
!pip install --quiet transformers accelerate gradio
```

---

## Model Bilgisi

```python
from transformers import pipeline

generator = pipeline(
    "text-generation",
    model = "microsoft/Phi-4-mini-instruct",
    torch_dtype ="auto",
    device_map="auto"
)
```

* Model, finansal alışkanlıklar konusunda kullanıcıya sorular sorması için sistem mesajıyla başlatılır:

```python
messages = [
  {"role": "system", "content": "You are a helpful and curious AI assistant that helps users reflect on their personal financial habits. You ask thought-provoking and insightful questions to better understand them."},
  {"role": "user", "content": "Please ask me three smart questions about my financial behavior to help me reflect and improve."}
]
```

---

## Sistem Çıktısı Örneği

```python
outputs = generator(
    messages,
    max_new_tokens=200,
    do_sample=True,
    temperature=0.7,
    return_full_text = False
)

print(outputs[0]["generated_text"])
```

Bu komut sonucunda model, kullanıcının kendi finansal kararlarını düşünmesini sağlayacak sorular üretir.

---

## Arayüz Bilgisi

Gradio ile hazırlanmış demo:

```python
def generate_text(prompt):
  messages = [
    {"role": "system", "content": SYSTEM_MESSAGE},
    {"role": "user", "content": prompt}
  ]
  outputs = generator(
    messages,
    max_new_tokens=100,
    do_sample=True,
    temperature=0.7,
    return_full_text = False
  )
  return outputs[0]["generated_text"]
```

Arayüz:

```python
demo=gr.Interface(
    fn=generate_text,
    inputs = gr.Textbox(label='Enter a prompt!'),
    outputs = gr.Textbox(label='Output'),
    title = "Financial AI Chatbot"
)

demo.launch()
```

---

## Gelecek Planları

* Hafıza sistemi ile sohbet geçmişi entegrasyonu (Langchain)
* Görsel finans raporları üretme yeteneği
* Daha doğru bilgiler sunmak için API veri entegrasyonu (TCMB, bankalar vb.)
* Mobil arayüz uyarlaması (Flutter/React Native)

---

> Hazırlayan: **Yusuf Ziya Demirel**
> Proje: DenizBank İlerisi Gençlik Bootcamp
> Amaç: Finansal bilinç kazandıracak sorularla kullanıcıyı desteklemek
> Model: `microsoft/Phi-4-mini-instruct`
