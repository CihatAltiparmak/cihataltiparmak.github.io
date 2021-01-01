---
title: "C Dilini Python İçinde Kullanma"
layout: post
ust: "yazılım"
---

Merhabalar,bu bölümde C ile yazılan fonksiyonları Pythonda nasıl kullanacağımızı incleyeceğiz. İlk olarak c dili ile bir şeyler yazalım.

hesap.c

{% highlight c %}
int toplama(int a, int b)
{
    return a+b;
}

int cikar(int a, int b)
{
    return a-b;
}
{% endhighlight %}										

Daha sonra aynı dizinde terminal veya cmd açın.Ben linux kullandıgım için terminalden anlatacağım.

Ve terminal veya cmd de aşagıdaki komutları verin:

Windows için:

{% highlight css %}
gcc -shared -Wl,-soname,adder -o hesapPaket.dll -fPIC hesap.c
{% endhighlight %}					
					

Linux için:

{% highlight python %}
gcc -shared -Wl,-soname,adder -o hesapPaket.so -fPIC hesap.c
{% endhighlight %}
					
					

Farkettiyseniz bulunduğumuz dizinde yeni bir dosya oluştu.Linux'da .so, windowsda .dll dir bu dosyanın uzantısı.Konumuza devam edelim.Gelin şimdi ise bu yeni oluşan dosyayı python scriptimizde nasıl kullanacağımızı görelim.Öyleyse aynı dizinde deneme.py adlı dosya oluşturalım ve şunları yazalım:

deneme.py

{% highlight python%}
from ctypes import cdll
hesapPartikulu = cdll.LoadLibrary("./hesapPaket.so") #Eger cdll.LoadLibrary("hesapPaket.so") yazarsanız hata alırsınız
#bu arada eğer windows kullanıyorsanız hemen yukarıdaki kodu hemen aşagıdaki gibi değiştirin.
#hesapPartikulu = cdll.LoadLibrary("./hesapPaket.dll")
print(hesapPartikulu.toplama(1, 2))
print(hesapPartikulu.cikar(15, 5))
{% endhighlight %}					
					

Kutlarım,tebrikler,python içinde c kullandınız :)

Sonraki yazılarda görüşmek üzere ...
