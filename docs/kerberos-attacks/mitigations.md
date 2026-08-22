# Mitigations

## AS-REP Roasting

* Kerberos pre-authentication aktif edilmelidir.
* Güçlü bir şifreleme algoritması kullanılmalıdır.
* Servis hesapları güçlü parolalar ile korunmalıdır.
* Servis hesapları şüpheli etkinlikler için izlenmelidir.

## Kerberoasting

* Uzun ve karmaşık parolalar kullanılmalıdır.
* Servis hesapları için parola rotasyonu ayarlanmalıdır.
* Servis hesapları çok yüksek ayrıcalıklara sahip olmamalıdır.

## Kerberos Delegations

* Mümkün olduğu yerde kısıtlanmamış delegasyon devre dışı bırakılmalıdır.
* Mümkün olduğu yerde kullanıcılar ve servis hesapları Protected Users grubuna eklenmelidir.
* Delegasyon lazım olmayan hesaplar için ACCOUNT IS SENSITIVE AND CANNOT BE DELEGATED seçeneği aktif edilmelidir.

## Golden Ticket

* En az ayrıcalık ilkesi uygulanmalıdır.
* Çok faktörlü kimlik doğrulama tercih edilmelidir.
* Mümkün olduğunca az yönetici hesabı kullanılmalıdır.
* Uç nokta algılama çözümleri kullanılmalıdır.
* Uzak bağlantı herkes tarafından kullanılabilen bir seçenek olmamalıdır.

## Silver Ticket

* Servis hesapları güçlü parolalar ile korunmalıdır.
* Servis hesapları için parola rotasyonu ayarlanmalıdır.
* Servis hesapları Administrators gibi ayrıcalıklı gruplara üye olmamalıdır.

## Pass-the-Ticket

* Mümkün olduğunca az yönetici hesabı kullanılmalıdır.
* Şüpheli şekilde TGT ve TGS bileti talep eden kullanıcılar kontrol edilmelidir.
