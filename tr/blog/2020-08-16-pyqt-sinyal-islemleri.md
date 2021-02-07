---
title: "PyQt5 Sinyal İşlemleri"
layout: default
ust: "yazılım"
---
Merhabalar, bu bölümde PyQt5 de sinyal oluşturma ve bu sinyali kullanmayı göreceğiz.Lafı uzatmadan işe koyulalım.

Önce PyQt5 i import edelim ve QObject oluşturalım.(Kavramı yanlış kullanmışsam özür diliyorum.)

{% highlight python  %}
from PyQt5.QtCore import *
class BunlarBenimSinyallerim(QObject):
    roketpatlasin = pyqtSignal()
{% endhighlight %}					
					

Sonra biz BunlarBenimSinyallerim adlı sınıfımızı örnekleyelim.

{% highlight python  %}
from PyQt5.QtCore import *
class BunlarBenimSinyallerim(QObject):
    roketpatlasin = pyqtSignal()

sinyal = BunlarBenimSinyallerim()
{% endhighlight %}					
					

İşte şimdi işin sonuna geliyoruz.Sinyal yayıldıgında çalışacak olan fonksiyonumuzu yazalım ve bu fonksiyonu sinyale bağlayalım.

{% highlight python  %}
from PyQt5.QtCore import *
class BunlarBenimSinyallerim(QObject):
    roketpatlat = pyqtSignal()

sinyal = BunlarBenimSinyallerim()

@pyqtSlot()
def on_roketPatladiginda():
    print("heeeey","Roket patlıyoooor...", "biri roketi patlatmak için sinyal yaydı!")

sinyal.roketpatlat.connect(on_roketPatladiginda)
{% endhighlight %}					
					

Şimdi gelin roketi patlatalım :)

{% highlight python  %}
from PyQt5.QtCore import *
class BunlarBenimSinyallerim(QObject):
    roketpatlasin = pyqtSignal()

sinyal = BunlarBenimSinyallerim()
@pyqtSlot()
def on_roketPatladiginda():
    print("heeeey","Roket patlıyoooor...", "biri roketi patlatmak için sinyal yaydı!")

sinyal.roketpatlat.connect(on_roketPatladiginda)

sinyal.emit()
{% endhighlight %}					
					

Gelin bir de sinyali parametreli yayalım.

Sinyalimiz çalıştıgında arabamız istediğimiz mesafeye gitsin.Ne dersiniz?Hadi işe koyulalım.

{% highlight python  %}
from PyQt5.QtCore import *
class BunlarBenimSinyallerim(QObject):
    arabayahukmet = pyqtSignal(int)

sinyal = BunlarBenimSinyallerim()
@pyqtSlot(int)
def on_arabayaSinyalVerdiginde(gidilecek_mesafe):
    print("Merhabalar, ben otomobil","bir sinyal aldım ve bu sinyal benim {} metre yol almamı emir buyurdu.".format(gidilecek_mesafe))

sinyal.arabayahukmet.connect(on_arabayaSinyalVerdiginde)

sinyal.emit(19)
{% endhighlight %}	

Sonraki yazılarda görüşmek üzere ...								

[<-- geri](../)
