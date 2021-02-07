---
title: "gtk ve Sinyal İşlemleri"
layout: default
ust: "yazılım"
---

Merhabalar, bu bölümde Gtk da sinyal oluşturma ve bu sinyali kullanmayı göreceğiz.

Önce GObject i import edelim ve GGObject oluşturalım.(Kavramı yanlış kullanmışsam özür diliyorum.)

{% highlight python  %}
from gi.repository import GObject
class BunlarBenimSinyallerim(GObject.GObject):
    def __init__(self):
    	GObject.GObject.__init__(self)

GObject.type_register(bunlarBenimSinyallerim)
GObject.signal_new("roketpatlat", bunlarBenimSinyallerim, GObject.SIGNAL_RUN_FIRST,GObject.TYPE_NONE, ())
{% endhighlight %}					
					

Sonra BunlarBenimSinyallerim adlı sınıfımızı örnekleyelim.

{% highlight python  %}					
from gi.repository import GObject
class BunlarBenimSinyallerim(GObject.GObject):
    def __init__(self):
    	GObject.GObject.__init__(self)

GObject.type_register(bunlarBenimSinyallerim)
GObject.signal_new("roketpatlat", bunlarBenimSinyallerim, GObject.SIGNAL_RUN_FIRST,GObject.TYPE_NONE, ())

sinyal = BunlarBenimSinyallerim()
{% endhighlight %}					
					

İşte şimdi işin sonuna geliyoruz.Sinyal yayıldıgında çalışacak olan fonksiyonumuzu yazalım ve bu fonksiyonu sinyale bağlayalım.

{% highlight python  %}					
from gi.repository import GObject
class BunlarBenimSinyallerim(GObject.GObject):
    def __init__(self):
    	GObject.GObject.__init__(self)

GObject.type_register(bunlarBenimSinyallerim)
GObject.signal_new("roketpatlat", bunlarBenimSinyallerim, GObject.SIGNAL_RUN_FIRST,GObject.TYPE_NONE, ())

sinyal = BunlarBenimSinyallerim()

def on_roketPatladiginda(object):
    print("heeeey","Roket patlıyoooor...", "biri roketi patlatmak için sinyal yaydı!")

ID = sinyal.connect("roketpatlat", on_roketPatladiginda)
{% endhighlight %}					
					

Şimdi gelin roketi patlatalım :)

{% highlight python  %}					
from gi.repository import GObject
class BunlarBenimSinyallerim(GObject.GObject):
    def __init__(self):
    	GObject.GObject.__init__(self)

GObject.type_register(bunlarBenimSinyallerim)
GObject.signal_new("roketpatlat", bunlarBenimSinyallerim, GObject.SIGNAL_RUN_FIRST,GObject.TYPE_NONE, ())

sinyal = BunlarBenimSinyallerim()

def on_roketPatladiginda(object):
    print("heeeey","Roket patlıyoooor...", "biri roketi patlatmak için sinyal yaydı!")

ID = sinyal.connect("roketpatlat", on_roketPatladiginda)
sinyal.emit()
{% endhighlight %}					
					

Şimdi o bagladığımız fonksiyonu blocklayalım.

{% highlight python  %}					
from gi.repository import GObject
class BunlarBenimSinyallerim(GObject.GObject):
    def __init__(self):
    	GObject.GObject.__init__(self)

GObject.type_register(bunlarBenimSinyallerim)
GObject.signal_new("roketpatlat", bunlarBenimSinyallerim, GObject.SIGNAL_RUN_FIRST,GObject.TYPE_NONE, ())

sinyal = BunlarBenimSinyallerim()

def on_roketPatladiginda(object):
    print("heeeey","Roket patlıyoooor...", "biri roketi patlatmak için sinyal yaydı!")

ID = sinyal.connect("roketpatlat", on_roketPatladiginda)
sinyal.emit("roketpatlat")
sinyal.handler_block(ID)
print("------")
sinyal.emit("roketpatlat")
print("------")
print("bak şu an roket patladı.")
print("sen görürsün sinyaldeki engeli bi kaldırayım,sen o zaman göreceksin...")
sinyal.handler_unblock(ID)
sinyal.emit("roketpatlat")
print("Kimse roketi patlatmama engel olamaz...")
{% endhighlight %}					
					

Gelin bir de sinyali parametreli yayalım.

Sinyalimiz çalıştıgında arabamız istediğimiz mesafeye gitsin.Ne dersiniz?Hadi işe koyulalım.

{% highlight python  %}					
from gi.repository import GObject
class BunlarBenimSinyallerim(GObject.GObject):
    def __init__(self):
    	GObject.GObject.__init__(self)

GObject.type_register(bunlarBenimSinyallerim)
GObject.signal_new("arabayahukmet", bunlarBenimSinyallerim, GObject.SIGNAL_RUN_FIRST,GObject.TYPE_NONE, (int, ))

sinyal = BunlarBenimSinyallerim()

def on_arabayaSinyalVerdiginde(object, gidilecek_mesafe):
    print("Merhabalar, ben otomobil","bir sinyal aldım ve bu sinyal benim {} metre yol almamı emir buyurdu.".format(gidilecek_mesafe))

ID = sinyal.connect("arabayahukmet", on_arabayaSinyalVerdiginde)
sinyal.emit("arabayahukmet", 19)
{% endhighlight %}					
					
Sonraki yazılarda görüşmek üzere ...

[<-- geri](../)
