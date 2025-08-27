# FinChatBot: Finansal Danışman Chatbot Projesi

Bu proje, Python ve doğal dil işleme teknolojilerini kullanarak geliştirilen bir finansal danışman chatbot sistemidir. Kullanıcılar, bu chatbot aracılığıyla kredi başvuru koşullarını öğrenebilir, faiz hesaplamaları yapabilir ve güncel kampanyalar hakkında bilgi alabilirler. Proje, metin tabanlı bir arayüzle çalışmakta olup gelecekte görsel arayüz entegrasyonu planlanmaktadır.

## İçindekiler

* [Genel Bakış](#genel-bakış)
* [Kullanılan Teknolojiler](#kullanılan-teknolojiler)
* [Sistem Bileşenleri](#sistem-bileşenleri)
* [Geliştirme Aşamaları](#geliştirme-aşamaları)

  * [1. Proje Dosya Yapısı](#1-proje-dosya-yapısı)
  * [2. Model Entegrasyonu](#2-model-entegrasyonu)
  * [3. Prompt Mimarisi ve Hafıza Sistemi](#3-prompt-mimarisi-ve-hafıza-sistemi)
  * [4. Kullanıcı Arayüzü](#4-kullanıcı-arayüzü)
  * [5. Testler ve Sonuçlar](#5-testler-ve-sonuçlar)
* [Gelecek Planları](#gelecek-planları)

---

## Genel Bakış

Bu proje, DenizBank İlerisi Gençlik Bootcamp kapsamında geliştirilmiştir. Amaç, yapay zeka destekli bir sistem kullanarak finansal bilgiye kolay erişim sağlamaktır. Chatbot, hem kredi şartlarını hem de genel finansal soruları yanıtlayabilmektedir.

## Kullanılan Teknolojiler

* **Python 3.10**
* **HuggingFace Transformers** (Qwen modeli)
* **Gradio** (Arayüz)
* **Accelerate** (GPU desteği)
* **CUDA 12.1** (GPU ile model yükleme)

## Sistem Bileşenleri

* **Chatbot Modeli:** HuggingFace'den indirilen Qwen modeli
* **Tokenizer:** Qwen'e uygun tokenizer
* **Prompt İşleme:** Hafıza temelli prompt oluşturma fonksiyonu
* **UI:** TextBox girişli, minimal Gradio arayüzü

## Geliştirme Aşamaları

### 1. Proje Dosya Yapısı

```
FinChatBot/
|— app.py
|— chatbot/
|   |— rag_chain.py
|   |— memory.py
|— environment.yml
|— README.md
```

### 2. Model Entegrasyonu

Model HuggingFace üzerinden otomatik olarak çekilir. CUDA destekli donanımlarda otomatik olarak GPU kullanımı sağlanır.

```python
model = AutoModelForCausalLM.from_pretrained(model_id, torch_dtype=torch.float16, device_map="auto")
```

### 3. Prompt Mimarisi ve Hafıza Sistemi

`build_prompt()` fonksiyonu ile kullanıcı mesajlarına dayalı bir sohbet geçmişi prompt'a entegre edilir.

### 4. Kullanıcı Arayüzü

Gradio tabanlı olarak geliştirilmiştir. Textbox girişli, çıktılar ise dinamik olarak text alanında sunulur.

```python
gr.Interface(fn=chat_function, inputs=gr.Textbox(), outputs=gr.Textbox()).launch()
```

### 5. Testler ve Sonuçlar

Model doğrulukla yanıt vermektedir. Güncel olarak kampanya bilgisi, kredi oranları gibi örnek veri setleriyle test edilmektedir.

## Gelecek Planları

* Gerçek veri seti ile entegrasyon (API ya da manuel input)
* Konu tabanlı yanıt sistemleri
* Gradio yerine React tabanlı arayüz
* Mobil versiyon (Flutter ya da React Native)
* Open Source GPT modelleriyle benchmark karşılaştırması

---

> Hazırlayan: **Yusuf Ziya Demirel**
> Proje: DenizBank İlerisi Gençlik Bootcamp
> Yöntem: RAG tabanlı Chatbot + LLM + Hafıza destekli tasarım
