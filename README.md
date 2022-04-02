# PostmanAPIAlistirmalarim
 Postman APı (arayüz) test alıştırmalarımı ve projelerimi paylaştığım repodur.

## Trello Uygulamasının Arayüzleri Kullanılarak Yaptığım Alıştırma
Postman aracı ve Trello API kullanılarak ***POST, PUT ve DELETE*** çağrı metotları ile istekler oluşturuldu. [Trello REST API](https://developer.atlassian.com/cloud/trello/rest/api-group-actions/) sitesi üzerinden gerekli Url, parametre, key ve token bilgileri elde edildi. Postman üzerinde yapılan istekler :
- Trello uygulamasında yeni bir Board (Pano) oluşturma
- Oluşturulan Panoya yeni bir liste ekleme
- Listeye yeni bir kart ekleme
- Oluşturulan kartı güncelleme
- Kartı silme 

***Path ve Query*** parametreleri oluşturularak global ve environment değişkenleri ile alıştırma dinamik hale getirildi. Ortam değişkenleri oluşturmak için ***Tests*** kısmında script kullanıldı.
```Javascript
let response = pm.response.json();
pm.environment.set("kartID", response.id);
```

```Javascript
let response = pm.response.json();
pm.environment.set("listID", response.id);
```

```Javascript
let response = pm.response.json();
pm.environment.set("panoID", response.id);
```

***Snıppets*** kısmındaki scripts incelendi. Her isteğin status code değerini Test Results kısmında alabilmek için ***Test*** alanına eklenen script :
```Javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

***Pre-request Script*** alanına arayüz çağrı öncesi koşulması gereken script kodları yazıldı. Her istek gönderiminde farklı isimde Board (Pano) oluşturulması dinamik hale getirildi. Pano isminin çağrı öncesi farklı bir datayı içermesi sağlandı.
```Javascript
pm.environment.set("panoAdi","UdemyTrelloTest " + parseInt(Math.random()*1000));
```

Test koşumunda testlerden beklenen sonucu ve gerçekleşen sonucu karşılaştırmak için Chai Kütüphanesi kullanıldı. Pano adı ve closed kısmı kontrol edildi. ***Tests*** alanına yazılan script :
```Javascript
pm.test("Trello deger kontrolu", function (){
    pm.expect(response.name).is.eql(pm.environment.get("panoAdi"));
    pm.expect(response.closed).to.be.false;
});
```
Yazdığım testleri koşmak için Collection Runner kısmını çalıştırdım. Postman testi otomatik şekilde koşulmuş oldu.

![Dinamik değişkenler ile test runner](https://user-images.githubusercontent.com/42176018/161377120-de4524a2-0ef8-4d66-b046-b1cdd3ad8e3b.png)

## Trello API ile Postman Projesi
Projede yapılan adımlar :
1. Trello üzerinde Pano oluştur.
2. Liste oluştur.
3. Kart oluştur.
4. Kart üzerine arayüz çağrısı ile Checklist (Kontrol Listesi) oluştur.
5. Label (Etiket) oluştur.

Key, token ve baseUrl değişkenleri oluşturuldu. Tests alanına ***Snippets scripts Status code:Code is 200*** eklendi. Bütün çağrılar Collection Runner ile koşuldu.

![Proje tests sonucu](https://user-images.githubusercontent.com/42176018/161377453-c3f27e37-25d9-49e8-8356-10bc865d900d.png)

## Otel Rezervasyon Projesi
Otel Rezervasyon uygulamasının [arayüz dökümanları](http://restful-booker.herokuapp.com/apidoc/index.html) kullanılarak GET, POST, PUT, PATCH ve DEL çağrı metotları ile Postman API testi gerçekleştirildi. 

Token oluşturma işlemi için body kısmına username ve password bilgileri girildi. (Raw --> JSON formatında)

Ortam ve global değişkenler oluşturuldu.
```Javascript
let response = pm.response.json();
pm.environment.set("token", response.token);
```
```Javascript
let response = pm.response.json();
pm.environment.set("bookingID",response.bookingid);
```
Pre-script ile totalprice alanı dinamik hale getirildi. Random sayıların totalprice kısmında oluşturulması sağlandı.
```Javascript
pm.environment.set("totalPrice", parseInt(Math.random()*1000));
```
totalprice alan kontrolü için Tests kısmına test script yazıldı.
```Javascript
let response = pm.response.json();
pm.test("totalPrice alanı doğru dönmelidir.", function(){
    pm.expect(response.totalprice).is.eql(pm.environment.get("totalPrice"));
});
```
Bütün çağrılar Collection Runner ile koşuldu.
![Otel Rezervasyon Test Sonucu](https://user-images.githubusercontent.com/42176018/161387888-10222234-dfb3-4461-a5c4-4d219a2c008b.png)
