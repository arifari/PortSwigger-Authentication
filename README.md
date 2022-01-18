# PORTSWIGGER WEB SECURITY-AUTHENTICATION LAB ÇÖZÜMLERİ
Web uygulamalarının en önemli parçalarından biri olan Authentication, belirli bir kullanıcı veya istemcinin kimliğini doğrulama işlemidir. Sitenin güvenliğini sağlayan ve her kullanıcı hesabı için sınırlar oluşturan bir kimlik doğrulama mekanizmasıdır. Kimlik doğrulama mekanizmaları zayıf olduğunda, brute force saldırılarına karşı yeterince koruma sağlanmadığında, mantık hataları ve uygulamadaki kodların güvenliği zayıf olduğunda Authentication zafiyeti meydana gelmektedir. 

Bu çalışmada bahsedilen Authentication zafiyeti için tüm laboratuvar çözümleri ele alunmıştır. Keyifli okumalar..
## Laboratuvarlar
[Lab 1: Username enumeration via different responses](https://github.com/arifari/PortSwigger-Authentication/blob/main/README.md#lab-1-username-enumeration-via-different-responses)

[Lab 2: Username enumeration via subtly different responses](https://github.com/arifari/PortSwigger-Authentication/blob/main/README.md#lab-2-username-enumeration-via-subtly-different-responses)

[Lab 3: Username enumeration via response timing]()

[Lab 4: Broken brute-force protection, IP block]()

[Lab 5: Username enumeration via account lock]()

## Lab 1: Username enumeration via different responses
Bu laboratuvarda bize bir kullanıcı adı ve parola listesi verilmektedir. Verilen bu wordlistler aracılığıyla brute force yöntemini kullanarak login olmaya çalışacağız. 

![](https://cdn-images-1.medium.com/max/800/1*OvfHX9mhVxeBlQhrhXMfSQ.png)

Laba erişim sağladıktan sonra My Account sayfasını açalım. 
Login panelinde random bir kullanıcı ve parola girelim ve isteği burp proxy ile inceleyelim.
Post metodu ile gönderilen bu isteği send ettiğimizde Invalid username cevabını almaktayız. Anlaşılan sayfada önce arif adında bir username var mı bunun doğrulaması yapılmaktadır. Böyle bir kullanıcı olmadığı için Invalid username cevabı dönmektedir.
Kullanıcı tespit etmek için önce isteği intrudera gönderelim.  username değerini $ tagları arasına alalım ve atak tipini Sniper seçelim.
Payload options bölümünden kullanıcı adı wordlistimizi tanımlayalım ve atağımızı başlatalım. Atak sonucunda bir kullanıcı adı (oracle) hariç diğer bütün kullanıcı adlarının Lenght değeri aynıdır. Ayrıca diğer kullanıcı adları için Invalid username cevabını alırken oracle adlı bir kullanıcı olduğu için Incorrect password yanıtını almaktayız. 
İsteği intruder üzerinden username=oracle&password=$1234$ şeklinde düzenleyerek bir de parola bilgisi için atak başlatıyoruz.
Atak sonucu, Status sütununda 1qaz2wsx parolası dışında bütün parolalara 200 durum kodunun yanıt olarak döndüğü görülmektedir. Kullanıcımızı ve parolamızı tespit ettiğimize göre login olalım.
Login olduktan sonra oracle kullanıcısına ait Email adresinin oracle@oracle.net olduğu bilgisi verilmektedir. Hesaba erişim sağladığımız için update edebiliriz. Bu laboratuvarın çözümü bu şekildedir.

## Lab 2: Username enumeration via subtly different responses
Bu laboratuvarda yapacağımız işlemler bir öncekiyle aynıdır. Sadece gönderdiğimiz isteklere biraz daha farklı yanıtlar alacağız.

![image](https://user-images.githubusercontent.com/77991445/149995139-66c57e4b-ee6f-4bd6-8aa9-5573503c8169.png)


Önceki bölümde isteği send ettiğimizde Invalid username yanıtını alıyorduk. Burada 'Invalid username or password.' yanıtını almaktayız. İsteği intrudera gönderip username wordlistimizde ki her kullanıcı için dönen yanıtı inceleyelim. username değerini $ tagları arasına alalım ve atak tipini Sniper seçelim.
