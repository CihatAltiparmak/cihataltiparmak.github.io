---
title: "Linux'da USB Giriş Çıkışını Anlama"
layout: post
ust: "yazılım"
---

### DBUS İLE GİREN USB BELLEKLERİ TESPİT ETME

Yukarıdaki yazılardan dbus ın matığını az çok anladınız.

Ben ilk olarak kodları vereyim.

{% highlight python  %}
import dbus

__all__ = ("getAllUnixDevices")

def getAllUnixDevices():
    busSession = dbus.SessionBus()
    obj = busSession.get_object("org.gtk.vfs.UDisks2VolumeMonitor", "/org/gtk/Private/RemoteVolumeMonitor")
    iface = dbus.Interface(obj, "org.gtk.Private.RemoteVolumeMonitor")
    _listdevice_ = iface.List()
    devices = []

    for i in _listdevice_[0]:
        for j in i:
            if isinstance(j, dbus.Dictionary):
                try:
                    #FIXME How do is select USB devices better without third party?
                    
                    path = deviceList("/org/freedesktop/UDisks2/block_devices/"+j["unix-device"].lstrip("/dev/"))
                    result = findUSBDevices(path)
                    if result[0]:
                        size = result[1].Get("org.freedesktop.UDisks2.Drive", "Size")
                        if size >= 10**12:
                            size = "%.0fTB" % round(size / 10**12)
                        elif size >= 10**9:
                            size = "%.0fGB" % round(size / 10**9)
                        elif size >= 10**6:
                            size = "%.0fMB" % round(size / 10**6)
                        elif size >= 10**3:
                            size = "%.0fkB" % round(size / 10**3)
                        else:
                            size = "%.0fB" % round(size)
                        devices.append([str(result[1].Get("org.freedesktop.UDisks2.Drive", "Vendor"))+" "+str(result[1].Get("org.freedesktop.UDisks2.Drive", "Model"))+" "+size, str(j["unix-device"]), result[1].Get("org.freedesktop.UDisks2.Drive", "Size")])
                        
                except KeyError:
                    pass
    return devices
        
    
def deviceList(objectPath):
    busSys = dbus.SystemBus()
    obj = busSys.get_object("org.freedesktop.UDisks2", objectPath)
    iface = dbus.Interface(obj, "org.freedesktop.DBus.Properties")
    return iface.Get("org.freedesktop.UDisks2.Block", "Drive")

def findUSBDevices(objectPath):
    busSys = dbus.SystemBus()
    obj = busSys.get_object("org.freedesktop.UDisks2", objectPath)
    iface = dbus.Interface(obj, "org.freedesktop.DBus.Properties")
    return (iface.Get("org.freedesktop.UDisks2.Drive", "ConnectionBus") == "usb", iface)

getAllUnixDevices()
{% endhighlight %}					
					

Asıl işi görecek olan getAllUnixDevices olacak.İlk olarak getAllUnixDevices çalışır bu kodda. Bu kodlardaki deviceList fonksiyonu bize usb olsun olmasın hardiskinden tut CD sine usb sine kadar bütün device ları elde eder(bütün device ları bir anda listelemez,Sadece findUSBDevices fonksiyonuna daha kolay parametre vermek için yazdım o fonksiyonu). findUSBDevices fonksiyonu, adı üstünde, deviceList fonksiyonundan elde edilen verileri kullanarak bize tuple veri tipi döner.tuple ın 1. değeri usb ise True, değilse False değeri alır.tuple ın 2. değeri parametre olarak verilen değerdir.Ama işimiz daha bitmemiştir.Bir de kullanıcıya bunu uygun formatta göstermek gerekiyor.Onu da getAllUnixDevices foksiyonu içinde zaten yapıyorum.

Öyleyse ilk inceleyeceğimiz kod parçası, aşağıda

{% highlight python  %}
[...]
def getAllUnixDevices():
    busSession = dbus.SessionBus()
    obj = busSession.get_object("org.gtk.vfs.UDisks2VolumeMonitor", "/org/gtk/Private/RemoteVolumeMonitor")
    iface = dbus.Interface(obj, "org.gtk.Private.RemoteVolumeMonitor")
    _listdevice_ = iface.List()
    devices = []
[...]
{% endhighlight %}					
					

Resimdeki List methodunu çalıştırın.Önünüze anlamsız sonuçlar gelecek.Öyleyse bu sonuçları anlamlı bir şekle sokmamız lazım.Linux da usb girdiğinde /devdizini altında sdb, sdc gibi sembolik dosyalar oluşur.İşte bunlar block_device lardır.

{% highlight python  %}
[...]
def getAllUnixDevices():
    busSession = dbus.SessionBus()
    obj = busSession.get_object("org.gtk.vfs.UDisks2VolumeMonitor", "/org/gtk/Private/RemoteVolumeMonitor")
    iface = dbus.Interface(obj, "org.gtk.Private.RemoteVolumeMonitor")
    _listdevice_ = iface.List()
    devices = []

    for i in _listdevice_[0]:
        for j in i:
            if isinstance(j, dbus.Dictionary):
                try:
                    #FIXME How do is select USB devices better without third party?
                    
                    path = deviceList("/org/freedesktop/UDisks2/block_devices/"+j["unix-device"].lstrip("/dev/"))
                    result = findUSBDevices(path)
[...]
{% endhighlight %}					
					

Gördüğünüz gibi bu anlamsız biçimi şekle sokmak için for döngüsü kullandım.Öncelikle _listdevice değişkeni bir liste veri tipidir.(Aslında öyle değil ama kafa karışıklığı olmasın) _liststore değişkeninin ilk değeri benim için daha iy. Çünkü sdb sdc gibi ifadeler o bölümde geçiyor. sonra o i ifadesini de for döngüsüne aldım.Çünkü _liststore[0] dan gelen i değişkenini de list gibi olduğu, ayrıca birden fazla device olduğu için böyle yaptım. gördüğünüz gibi önce deviceList fonksiyonunu, sonra deviceList fonksiyonundan aldığım değişkeni findUSBDevices değişkenine vererek o device ın usb olup olmadığını kontrol ettim.

{% highlight python  %}
[...]
def getAllUnixDevices():
    busSession = dbus.SessionBus()
    obj = busSession.get_object("org.gtk.vfs.UDisks2VolumeMonitor", "/org/gtk/Private/RemoteVolumeMonitor")
    iface = dbus.Interface(obj, "org.gtk.Private.RemoteVolumeMonitor")
    _listdevice_ = iface.List()
    devices = []

    for i in _listdevice_[0]:
        for j in i:
            if isinstance(j, dbus.Dictionary):
                try:
                    #FIXME How do is select USB devices better without third party?
                    
                    path = deviceList("/org/freedesktop/UDisks2/block_devices/"+j["unix-device"].lstrip("/dev/"))
                    result = findUSBDevices(path)
                    if result[0]:
                        size = result[1].Get("org.freedesktop.UDisks2.Drive", "Size")
                        if size >= 10**12:
                            size = "%.0fTB" % round(size / 10**12)
                        elif size >= 10**9:
                            size = "%.0fGB" % round(size / 10**9)
                        elif size >= 10**6:
                            size = "%.0fMB" % round(size / 10**6)
                        elif size >= 10**3:
                            size = "%.0fkB" % round(size / 10**3)
                        else:
                            size = "%.0fB" % round(size)
                        devices.append([str(result[1].Get("org.freedesktop.UDisks2.Drive", "Vendor"))+" "+str(result[1].Get("org.freedesktop.UDisks2.Drive", "Model"))+" "+size, str(j["unix-device"]), result[1].Get("org.freedesktop.UDisks2.Drive", "Size")])
                        
                except KeyError:
                    pass
    return devices
[...]
{% endhighlight %}					
					
{% highlight python  %}
if result[0]
{% endhighlight %}

doğruysa,yani o cihaz usb ise

{% highlight python  %}size = result[1].Get("org.freedesktop.UDisks2.Drive", "Size"){% endhighlight %}

kod parçasıyla o usb nin kaç gb, terabayt , mb olduğunu bulmak için bu kod parçasından değer alıyorum.Ama aldığım değer sadece usb nin kaç bayt olduğunu veriyor.Bizim onu anlamlı bir veriye dönüştürmemiz lazım. if bloğu içnde round ile değerleri yuvarlıyorum.Yoksa biraz anlamsız oluyor.

En sonunda ise en başta oluşturduğum device adlı listeye bu verileri ekliyorum.append ile kattığımız verileri de bir inceleyelim.

{% highlight python  %}str(result[1].Get("org.freedesktop.UDisks2.Drive", "Vendor")){% endhighlight %}

Cihazın vendorunu veriyor.Sandisk, Kingston vs.

{% highlight python  %}str(result[1].Get("org.freedesktop.UDisks2.Drive", "Model")){% endhighlight %}

Burada o cihazın modelini bastırıyorum.

{% highlight python  %}str(j["unix-device"]){% endhighlight %}

Bize usb için /dev/ dizini altında oluşturulmuş sembolik dosya patikasını verir.

{% highlight python  %}result[1].Get("org.freedesktop.UDisks2.Drive", "Size"){% endhighlight %}

Bu da bize usb uzunluğunu bayt cinsinden verir

Aslında bu scripti kendi projem için yazmıştım ama başkalarına yararlı olmak adına bu scripti anlatmak istedim.Kafanıza karışık gelen yerler olabilir.Sıkıntı yapmayın.Olmadı benle iletişime geçin.

Sonraki yazılarda görüşmek üzere ...
