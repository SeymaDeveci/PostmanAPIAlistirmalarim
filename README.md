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
```
let response = pm.response.json();
pm.environment.set("kartID", response.id);
```

```
let response = pm.response.json();
pm.environment.set("listID", response.id);
```

```
let response = pm.response.json();
pm.environment.set("panoID", response.id);
```

***Snıppets*** kısmındaki scripts incelendi. Her isteğin status code değerini Test Results kısmında alabilmek için ***Test*** alanına eklenen script :
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

***Pre-request Script*** alanına arayüz çağrı öncesi koşulması gereken script kodları yazıldı. Her istek gönderiminde farklı isimde Board (Pano) oluşturulması dinamik hale getirildi. Pano isminin çağrı öncesi farklı bir datayı içermesi sağlandı.
```
pm.environment.set("panoAdi","UdemyTrelloTest " + parseInt(Math.random()*1000));
```

Test koşumunda testlerden beklenen sonucu ve gerçekleşen sonucu karşılaştırmak için Chai Kütüphanesi kullanıldı. Pano adı ve closed kısmı kontrol edildi. ***Tests*** alanına yazılan script :
```
pm.test("Trello deger kontrolu", function (){
    pm.expect(response.name).is.eql(pm.environment.get("panoAdi"));
    pm.expect(response.closed).to.be.false;
});
```
Yazdığım testleri koşmak için Collection Runner kısmını çalıştırdım. Postman testi otomatik şekilde koşulmuş oldu.

![Dinamik değişkenler ile test runner](https://user-images.githubusercontent.com/42176018/161377120-de4524a2-0ef8-4d66-b046-b1cdd3ad8e3b.png)
