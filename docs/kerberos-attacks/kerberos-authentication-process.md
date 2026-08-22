# Kerberos Authentication Process

## Entities

![](assets/01.png)

* Key Distribution Center (KDC)
* Kullanıcı
* Servis

## Tickets

![](assets/02.png)

1. Kullanıcı KDC üzerinden bir bilet talep eder. Bu bilet Ticket Granting Ticket (TGT) olarak adlandırılır. TGT kullanıcı hakkındaki bilgileri içerir. Bu bilgiler Privilege Attribute Certificate (PAC) içerisinde tutulur.
2. Kullanıcı bir servise erişmek istediği zaman KDC bünyesine TGT sunar. KDC biletin geçerliliğini kontrol eder ve kullanıcıya bir Ticket Granting Service (TGS) bileti sağlar.
3. Kullanıcı erişmek istediği servise TGS bileti sunar. Servis biletin geçerliliğini kontrol eder ve kullanıcı servise erişebilir.

## Ticket Protection

![](assets/03.png)

1. TGT, KRBTGT gizli <span style="color:red">anahtarı</span> ile şifrelenir. Böylece kullanıcı, kendisi hakkındaki bilgileri okuyamaz veya değiştiremez.
2. TGS bileti, servis gizli <span style="color:green">anahtarı</span> ile şifrelenir. Böylece kullanıcı, TGS bileti içerisinde yer alan bilgiler için bir değişiklik yapamaz.

## Authentication Service (AS)

### AS-REQ

!!! warning

    Bu aşama pre-authentication olarak adlandırılır. Devre dışı bırakılması durumunda doğrulayıcı kullanılmaz.

![](assets/04.png)

Kullanıcı, kimliğini ispat etmek için AS-REQ talebine aşağıda verilen bilgileri ekler:

* Kullanıcı adı.
* Doğrulayıcı. Kullanıcının kendi gizli <span style="color:cyan">anahtarı</span> ile şifrelediği anlık zaman damgasıdır.

KDC, kullanıcıya ait gizli anahtarı bünyesinde bulunan anahtar dizininde araştırır ve bulduğu anahtar ile doğrulayıcı şifresini çözer.

### AS-REP

![](assets/05.png)

Kullanıcı ile KDC arasında bir oturum <span style="color:purple">anahtarı</span> oluşturulur. Haberleşmeyi güvence altına almak için bu anahtar kullanılır. Kerberos [stateless](https://en.wikipedia.org/wiki/Stateless_protocol) bir protokol olduğu için bu anahtar herhangi bir yerde saklanmaz.

AS-REP yanıtı aşağıda verilen kısımlardan oluşur:

* Oturum <span style="color:purple">anahtarı</span>. Kullanıcı gizli <span style="color:cyan">anahtarı</span> ile korunur.
* TGT. KRBTGT gizli <span style="color:red">anahtarı</span> ile korunur. PAC ve kopya oturum <span style="color:purple">anahtarı</span> içerir.

## Ticket Granting Service (TGS)

### TGS-REQ

![](assets/06.png)

* Doğrulayıcı. Oturum <span style="color:purple">anahtarı</span> ile korunur.
* TGT. KRBTGT gizli <span style="color:red">anahtarı</span> ile korunur.
* Service Principal Name (SPN). Erişilecek olan servis adını ifade eder.

### TGS-REP

!!! warning

    TGS-REQ talebindeki doğrulayıcı geçerliliği TGT içerisinde bulunan kopya oturum <span style="color:purple">anahtarı</span> ile kontrol edilir.

![](assets/07.png)

Kullanıcı ile servis arasında yeni bir oturum <span style="color:orange">anahtarı</span> oluşturulur.

TGS bileti aşağıda verilen kısımlardan oluşur:

* SPN.
* PAC. Servis gizli <span style="color:green">anahtarı</span> ile korunur.
* Kopya oturum <span style="color:orange">anahtarı</span>. Servis gizli <span style="color:green">anahtarı</span> ile korunur.

Tüm bu bilgiler kullanıcı ile KDC arasındaki oturum <span style="color:purple">anahtarı</span> ile korunur.

## Application Request (AP)

### AP-REQ

![](assets/08.png)

* Doğrulayıcı. Oturum <span style="color:orange">anahtarı</span> ile korunur.
* TGS bileti.

### AP-REP

!!! warning

    AP-REQ talebindeki doğrulayıcı geçerliliği TGS bileti içerisinde bulunan kopya oturum <span style="color:orange">anahtarı</span> ile kontrol edilir.

1. Oturum <span style="color:orange">anahtarı</span> ile şifrelenmiş bir doğrulayıcı AP-REP yanıtına eklenerek kullanıcıya gönderilir.
2. Kullanıcı yanıtın servisten geldiğini doğrular ve servise erişebilir.
