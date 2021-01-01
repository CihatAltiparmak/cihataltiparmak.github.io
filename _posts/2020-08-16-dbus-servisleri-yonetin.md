---
title: "dbus İle Servisleri Yönetin"
layout: post
ust: "yazılım"
---
Merhabalar, bu bölümde dbus modülünün kullanımından bahsedeceğiz.Lafı uzatmadan işe koyulalım.

> Uyarı: d-feet yazılımını kurun,zorunlu olmasa bile olmazsa olmazlardan.

### KURULUM

{% highlight sh %}
apt install d-feet
{% endhighlight %}			
					

DBUS İLE BİLDİRİM GÖSTERELİM

İlk olarak d-feet yazılımını çalıştıralım.

![dbus1](./img/dbus_ile_servisleri_yonetin/1.png "dbus1")

Daha sonra en üstte SystemBus ve SessionBus yazan iki buton var.Bunlardan Session Bus olanına basın.

Daha sonra arama çubuğuna notifi yazın ve org.freedesktop.notifications olana tıklayın.Aşağıda bir resim göreceksiniz.Ben orada tıkladım.

![dbus2](./img/dbus_ile_servisleri_yonetin/2.png "dbus2")

Sağ tarafta /org/Cinnamon gibi ifadeler var.O ifadelerden /org/freedesktop/Notifications olana tıklayın.

Orada Interfaces bölümü altında bir takım seçenekler daha var.Oradan da org.freedesktop.Notifications a tıklayın.

Karşınıza Methods ve Signals diye iki menu çıkacak.Biz Methods menusu altındaki Notify ye tıklayacağız.Karşınıza aşağıdaki gibi bir şey çıkacak.

![dbus3](./img/dbus_ile_servisleri_yonetin/3.png "dbus3")

Çıkan o popup menudeki yöntem girdisi alanına aşagıdakileri yazın.Ve çalıştır butonuna tıklayın.Ve sonucu görün.

{% highlight sh %}"",1,"", "Bildirimlerin Başlık Kısmı","Buyrun size bildirim kutucuğu.Güle güle notifications baloncuğunu görün : )",[],{},2{% endhighlight %}				
					

Buyrun ben yaptım ve sonuç:

![dbus4](./img/dbus_ile_servisleri_yonetin/4.png "dbus4")

İçinizden programlama diliyle bunu yap dediğinizi duyar gibiyim. Merak etmeyin. Asıl şimdi başlıyoruz.

ilk olarak denemeNotify.py dosyası oluşturup aşağıdakileri yazın.

{% highlight python  %}
import dbus
bus = dbus.SessionBus() #d-feet de ilk yaptıgımız şeyi hatırlayın
obje = bus.get_object("org.freedesktop.Notifications", "/org/freedesktop/Notifications") #İkinci ve üçüncü adımları hatırlayın.
iface = dbus.Interface(obje, "") #Hani hatırlıyor musunuz,Interface menusu demiştik...
iface.Notify("",1,"", "Bildirimlerin Başlık Kısmı","Buyrun size bildirim kutucuğu.Güle güle notifications baloncuğunu görün : )",[],{},2)
{% endhighlight %}									
					

Bu kodları çalıştırdığınızda da aynı sonucu alacaksınız.

### DBUS İLE EKRAN KİTLEYELİM

İlk olarak yine d-feet i çalıştıralım.

![dbus5](./img/dbus_ile_servisleri_yonetin/5.png "dbus5")

> Hangi servisi kullanmalıyım?

    - org.cinnamon.ScreenSaver(ben linux mint cinnamon kullanıyorum.Sizde başka türlü çıktı olabilir.)
    - org.freedesktop.ScreenSaver

Cinnamon lu olanı tercih edin.(Çünkü nedense org.freedesktoplu olanı çalıştıramadım.)

Oradan cinnamon lu olanı seçtikten sonra sağdaki menuye gelecek olan /org/cinnamon/ScreenSaver seçerseniz karşınıza aşağıdaki gibi bir resim peyda olacak.

![dbus6](./img/dbus_ile_servisleri_yonetin/6.png "dbus6")

Tabi çaktırmayın.Ben orada sağ menude bulunan Interface adlı menunun altındaki org.cinnamon.ScreenSaver a da tıkladım ve kullanabileceğim methodları görmüş oldum.

İşte o methodlar arasında Lock adlı bir method var. Ona çift tıklayın. Aşağıdaki gibi bir resim görünecek.

![dbus7](./img/dbus_ile_servisleri_yonetin/7.png "dbus7")

İşte şimdi tek yapacağınız şey, Yöntem Girdisi Bölümüne

{% highlight sh  %}
"Bu Ekran Kitlendiğinde Gösterilecek Olan Mesaj"
{% endhighlight %}										
					

yazmak ve alt sağda bulunan çalıştır butonuna basmak. Sonucu bilgisayarınızda görebilirsiniz.

Gelin bunu koda dökelim.

{% highlight python  %}
import dbus

bus = dbus.SessionBus()
obje = bus.get_object("org.cinnamon.ScreenSaver", "/org/cinnamon/ScreenSaver")
iface = dbus.Interface(obje, "/org/cinnamon/ScreenSaver")
iface.Lock("Bu Ekran Kitlendiğinde Gösterilecek Olan Mesaj")
{% endhighlight %}

Sonraki yazılarda görüşmek üzere ...					
