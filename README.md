# NewMind AI bitirme projesi.

(! Projeyi dökümente edin denildiği için bir projeyi ve kodu anlatan bir pdf hazırladım. Cohort'da ekran görüntüsü kısmına ekledim.)

İlk olarak bir .env dosyası ouşturulup şu kısımlar doldurulmalıdır =>

````
OPENAI_API_KEY=
NEO4J_URI=
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=-
````
Sanal ortam kullanmak python projeleri için önerilir. 

Proje ana dizinine gidilip bu komutlar çalıştırılabilir. 1. komut ortamı oluşturur. 2. komut ortamı aktifleştirir.
````
python -m venv venv
venv\Scripts\activate
````

Projemizin klasör yapısına bakalım. 

 ![image](https://github.com/user-attachments/assets/3980038c-2fb8-4e8c-927a-82dda106bb9b)


Burada create_kg.py dosyası denizcilik üzerine olan pdf’lerden neo4j de bir knowledge graph oluşturdu. OCR kullanımı için de ocr.py adında bir dosya oluşturdum. Chatbot’umuzun kodları ise chatbot klasörü altında bulunuyor.  Zaten tüm dosyaları ayrıntılı şekilde açıkladım. 

Projemizin başlaması çok basit bot.py’nin bulunduğu dizine gidip ```streamlit run bot.py``` yazarak projeyi başlatıyoruz.
 
 ![image](https://github.com/user-attachments/assets/832c18d4-bb76-4b31-b795-7bc32678cc43)


Şimdi projemizi denemek için chatbot’a sorular soralım.

Rastgele isteklerde bulunacağım. 

Ever Given adlı geminin bayrağı nedir?

Bu şekilde bir soru soracağım. Bu bilgi graph modelimizde bulunmuyor bakalım nasıl cevap verecek ?

![image](https://github.com/user-attachments/assets/0b83e732-109c-4434-ac2a-7e0fe7847d3e)
 
![image](https://github.com/user-attachments/assets/b2a2d6fa-f8f9-49c5-be76-1b935c06c50d)

 

Evet olmayan bir şey hakkında saçmalamadı. 

Şimdi bir label’a ait kaç node varmış ona bakalım.

Characteristic label’ına sahip 5 tane düğüm var. Property olarak modelimiz çok saçma evet çünkü bir şema vermedim. Bu sebeple name proprty’si yerine id kullanılmış o sebeple id bilgisini isteyeceğim. 
 
 
![image](https://github.com/user-attachments/assets/d3ce7b44-bf4d-4d91-a2b0-29d6f0c39e98)

 ![image](https://github.com/user-attachments/assets/b0139833-9634-412c-bc1b-eadb886edbf6)

![image](https://github.com/user-attachments/assets/6b756801-4bcb-4c7f-a189-a7c9219a4188)


Burada hiçbir bilgi getirmedi. Bunu kontrol etmeliyim. 

Cypher.py dosyasındaki  template’de example kısmını referans alıyordu ve o kısımı kaldırıp tekrardan sordum.



CYPHER_GENERATION_TEMPLATE = """Task:Generate Cypher statement to query a graph database.
Instructions:
Use only the provided relationship types and properties in the schema.
Do not use any other relationship types or properties that are not provided.
Only include the generated Cypher statement in your response.

Always use case insensitive search when matching strings.

Schema:
{schema}

The question is:
{question}"""


![image](https://github.com/user-attachments/assets/be9324e8-b06b-4276-8fa9-1eef33868d33)

 
Bu sefer güzel cevap verdi. 
Console’da şu şekilde ->

 ![image](https://github.com/user-attachments/assets/dc44d798-16af-42a5-80a0-ae2a4785ed46)


Bakalım example olmadan şu sorumuza ne cevap verecek ?

Kaç tane Türk bayraklı gemi var ?

 ![image](https://github.com/user-attachments/assets/53ffd963-b23b-4803-8acd-6798b30f6fad)

![image](https://github.com/user-attachments/assets/1265b57e-cc41-4e2c-9ea1-43c5b4a6a0d7)


Bu soruya cevap veremiyorum şeklinde yazdı. Template de de bunu belirtebiliriz aslında. Bunu da deneyeceğim. 

Console -> 

 
![image](https://github.com/user-attachments/assets/097b8904-58a0-40b2-8d94-a8c4f5174ab0)

![image](https://github.com/user-attachments/assets/28c424e1-3f9d-445d-af42-d11df09514fa)


Şimdi de 1. Tool’u kullanalım. 

Bir matematik sorusu soracağım.

Diferansiyel sorusu sorayım.

 ![image](https://github.com/user-attachments/assets/ef96d6da-4903-4b08-ab68-086bd12e83d7)


Burada biz prompt’umuda denizcilik harici cevap verme demiştik. Bunu unutmuştum bu cevap da güzeldi.

Burada agent’a talimat vermiştik. Başarılı şekilde uygulamış. Sadece denizcilik üzerine bilgi ver demiştik. 

 ![image](https://github.com/user-attachments/assets/abc51271-5fd9-4078-82cf-0d961261fc9e)


O zaman denizcilik üzerine soru soralım. 
2025 itibari ile Türkiye’nin kaç tane savaş gemisi var. 

![image](https://github.com/user-attachments/assets/5f11595c-92b1-4eb4-a484-40fae526e6dc)

 

Bu sefer Bu sorumuz ile 2. Tool’umuzu kullandık. Ama web araması için de bir tool oluşturmuş olsaydık. Bu srouya da cevap verebilirdik. Bu soru ile 2. Tool’u da kullanmış olduk yani şu tool -> Maritime News Search

![image](https://github.com/user-attachments/assets/53c1aad9-0c7d-498d-b23f-b0a5b16bb527)

 
Console ->
 
![image](https://github.com/user-attachments/assets/00372bf0-156b-4e5c-9247-c0fd3f94f546)


3.	ve 3. Tool’u kullandık şimdi de 1. Tool’u kullanalım. 

1.	Tool’u kullanmak için bile chatbot’a soru sordum. Şunu söyledi ->

![image](https://github.com/user-attachments/assets/b161ce60-987b-4c36-9760-225c3aaaf496)

 
Console ->
 
![image](https://github.com/user-attachments/assets/ae70ba83-9c3b-45b0-a492-995076813f63)

Ardından aynı soruyu sordum ->

 ![image](https://github.com/user-attachments/assets/b204c16e-1920-4776-aea0-d5c13fa18d21)


Console ->
 
![image](https://github.com/user-attachments/assets/d50ac209-773c-4fa9-89e6-ec1763443ed5)

Ama görüldüğü gibi yine 3. Tool kullanıldı çünkü graph ile ilgili cypher sorgusu yapılması gerektiğini anladı. 3. Tool’u kullandığını söyledim. 

  ![image](https://github.com/user-attachments/assets/7955583b-83a4-4aa1-8037-8cb7958e19dd)


Console ->

 ![image](https://github.com/user-attachments/assets/bff0754f-848c-4ddf-afd5-6825bdcb0283)


Bu sefer de bir tool kullanmadı.

Direkt olarak 1. Tool’u kullan dedim ->

 ![image](https://github.com/user-attachments/assets/23923586-ade4-4684-9cb3-7e5d2c4d1917)


Console ->

 ![image](https://github.com/user-attachments/assets/3e2ed4f7-538e-4c0d-8cce-10bf4ce956ec)


Burada artık General Chat’i kullandık. Fark ettiysek benim sorduğum soruyu tekrardan sordu ama bu sefer kendisi 1. Tool’u kullandı. 

Bu sefer şu soruyu sorarsak 1. Tool’u sorumuz ile kullanır  Denizcilik hakkında genel bir bilgi verebilir misin ? 

![image](https://github.com/user-attachments/assets/0b8cea36-1cdf-4db3-bc45-f62f8d5b4f8c)



Console ->

![image](https://github.com/user-attachments/assets/d25c3979-e9aa-4760-9f5b-f5ed470167ca)
 
Bu şekilde genel bir test de yapmış olduk. 

İyi çalışmalar.
