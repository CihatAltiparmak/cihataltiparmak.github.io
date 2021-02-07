---
title: "configparser Modülü ve Kullanımı"
layout: default
ust: "yazılım"
---

Merhabalar, bu bölümde configparser ile cfg, ini dosyaları oluşturacağız.

### configparser İle .cfg Veya .ini Dosyalarından Veri Okuma

İlk olarak denemeConfig.py adlı dosya oluşturalım ve içine şunları yazalım.

{% highlight python  %}
from configparser import ConfigParser

c = ConfigParser()
{% endhighlight %}					
					

diyerek ConfigParser sınıfımızı örnekledik.Şimdi sıra dosyadan veri okumada.Ama önce aynı dizinde deneme.cfg adlı bir dosya daha oluşturalım.

deneme.cfg

{% highlight python  %}
[elma]
fiyat= 7
kalite=Niğde elması kalitesinde,yani muhteşem
ömür="1 hafta"

[incir]
fiyat=5
kalite=on numara 4 yıldız
ömür=3 gün
{% endhighlight %}					
					

Şimdi ise bu dosyadaki bilgilere erişelim.

{% highlight python %}
from configparser import ConfigParser

c = ConfigParser()
c.read("deneme.cfg")
{% endhighlight %}					
					

Şimdi ise verilere erişelim.

{% highlight python %}
from configparser import ConfigParser

c = ConfigParser()
c.read("deneme.cfg")
print("elma yi tanıyalım...")
print(c["elma"])
print(c["elma"]["fiyat"], c["elma"]["kalite"])
print("------------")
print("inciri tanıyalım")
print(c["incir"])
print(c["incir"]["fiyat"], c["incir"]["kalite"])
{% endhighlight %}					
					

### configparser İle .cfg Veya .ini Dosyalarına Veri Yazma

İlk olarak denemeConfig.py adlı dosya oluşturalım ve içine şunları yazalım.

{% highlight python %}					
from configparser import ConfigParser

c = ConfigParser()
{% endhighlight %}					
					

diyerek ConfigParser sınıfımızı örnekledik.Şimdi sıra dosyadan veri yazmada.

{% highlight python  %}
from configparser import ConfigParser

c = ConfigParser()
c["AYARLAR"] = {
	"parlaklık" : 20,
	"ses düzeyi" : 130,
	"durum" : False
}

c["OYUN SECENEKLERİ"] = {
	"fare yönü" : "sağ"
}
{% endhighlight %}					
					

Şimdi ise bu verileri bir dosyaya kaydedelim.

{% highlight python %}
from configparser import ConfigParser

c = ConfigParser()
c["AYARLAR"] = {
	"parlaklık" : 20,
	"ses düzeyi" : 130,
	"durum" : False
}

c["OYUN SECENEKLERİ"] = {
	"fare yönü" : "sağ"
}

with open("deneme.cfg", "w") as f:
	c.write(f)
{% endhighlight %}					
					

Dizinde oluşan deneme.cfg adlı dosyayı açtığınızda şunları göreceksiniz:

{% highlight python %}
[AYARLAR]
parlaklık = 20
ses düzeyi = 130
durum = False

[OYUN SECENEKLERİ]
fare yönü = sağ
{% endhighlight %}					
					
Sonraki yazılarda görüşmek üzere ...

[<-- geri](../)
