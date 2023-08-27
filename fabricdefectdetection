#Görüntü İşleme Tekniği kullanarak kumaşlarda kusur tespiti 
# Bu proje kapsamında Kumaş Resimlerinin renk dağılımı ve renk bilgisini daha iyi anlamak için 2 farklı renk tonu ile ifade edilir. 
# Bu resimlerin renklerinin sayısal değerleri grafik üzerinde gösterilir. Grafiğin simetriliği bozan bir yer tespit edilirse hata tespit edilmiş olur.
import numpy as np
import matplotlib.pyplot as plt
import cv2 as cv

resm1 = cv.imread("renkhata.png") #orjinal resimi temsil eder
resim= cv.imread("renkhata.png",0) #işlenecek resini temsil eder.
blr1 = cv.blur(resim,(15,15)) #görüntüdeki gürültüleri gidermek için görüntüyü bulanıklaştırır.
ret, thresh1 = cv.threshold(blr1, 50, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)# gürültüsü azaltılmış resmi siyah ve beyaza dönüştürür.

kernel = np.ones((7,7),np.uint8)
dilation = cv.erode(thresh1,kernel,iterations=1) #resmin ön planını arka planından ayırmak için bu fonksiyon kullanılır.

    

x,y = dilation.shape #x sütun, y #satır
a = 0
orta = []
k = [] #k arrayi olusturuluyor
kucuk = 0 #en kucuk değeri ifade eden değişken
buyuk = 0 #en buyuk deüeri ifade eden değişken
miny = [] #grafikte bulunan minimum noktaları depolayan dizi
maxy = [] #grafikte bulunan maksimum noktaları depolayan dizi
maxyn = []
for i in range(x): #sütun sayısı kadar yani 739 tane döngü komutu
    a = 0
    ort = 0
    for j in range(y): #satır sayısı kadar 737 tane döngü komutu
        a = a + dilation[i][j] #i. sütun için tüm satırların piksel değerlerinin toplamını alan değişken
    ort = a / y  #tüm satırların piksel değerlerinin toplamını y satır adetine bölüyor .herbir sütunun ortalama piksel değeridir.
    orta.append(ort) #her bir sütunun ortalama piksel değerlerini depolayan dizi.
buyuk = orta[0] #en büyük sayinin ilk değeri dizideki ilk eleman olsun, döngü boyunca değişecektir.
kucuk = orta[0] #en kücük sayinin ilk değeri dizideki ilk eleman olsun, döngü boyunca değişecektir.

siyahlik = 0 #piksel değeri sıfır olan kumaş sayısını temsil eder
for u in range(len(orta)-1): #orta arrayinin eleman sayısından 4 eksik kadar döngü başlasın
        if orta[u] == 0: #piksel değeri sıfır ise 
                siyahlik = siyahlik + 1 #siyahlik adlı değişken artsın
                if siyahlik > 725: #sıfır oranı 725 den büyük ise
                        print("HATA ORANI YOKTUR. KUMAŞ HATASIZDIR")
                        images = [resm1,dilation] 
                        for i in range(2):
                           plt.subplot(2,2,i+1),plt.imshow(images[i],'gray',vmin=0,vmax=255) #resimleri göstermek için
   
                           plt.xticks([]),plt.yticks([])
                        plt.show()

 #orta adli dizinin grafik temsilinin maksimum ve minimum noktalarını bulma 
for t in range(len(orta)-1): # orta adli dizinin eleman sayısı kadar dongu baslasın
        t = t + 1
        if t < (len(orta)-1):
            if orta[t-1] > orta[t]: #t-1.eleman t den büyük ise yani azalan 5 4 gibi
                   if orta[t] > orta[t+1]: #t.eleman t+1 den büyükse azalmaya devam 4 3 gibi
                         buyuk = buyuk #büyük sayi büyüklüğünü koruyacak
                   elif orta[t] < orta[t+1]: #t. eleman t+1 den kucuk ise orada min var 4 7 gibi
                           kucuk = orta[t] #en kucuk olan değer t.eleman olacak. 
                           miny.append(kucuk) #minimum nokta miny adli diziye eklenir
                   elif orta[t] == orta[t+1]: #t.eleman t+1.ye esitse min olarak alabiliriz
                           kucuk = orta[t+1] #aynı sayı olacağından miny adli diziye eklemeye gerek yok.                
            elif orta[t-1] < orta[t]: #t-1.eleman t den kucuk ise yani artan 3 6 gibi
                   if orta[t] > orta[t+1]: #t.eleman t+1 den buyuk ise 6 4 gibi azalmaya baslamıs max olabilir
                             buyuk = orta[t] 
                             maxy.append(buyuk) #maksimum noktanın maxy adlı diziye eklenmesi
                             maxyn.append(t) #maksiumum noktanın indexinin maxyn adlı değişkene eklenmesi
                   elif orta[t] < orta[t+1]: # 2 3den kucuk ise 6 12 
                             kucuk = kucuk 
                   elif orta[t] == orta[t+1]: #2 3 e esit ise ise    
                             buyuk = orta[t+1]      
            elif orta[t-1] == orta[t]: #1 2 ye esit ise   5 5     
                 if orta[t] > orta[t+1]: #2 de  3 ten  buyuk ise yani azalan 5 3
                         buyuk = orta[t] 
                         maxy.append(buyuk)
                         maxyn.append(t)  
                 elif orta[t] < orta[t+1]: #2 de 3 den kucuk ise yani artan.. 5 7
                          kucuk = orta[t]
                          miny.append(kucuk)
                   
                   
fark1 = []
farkp1 = []
farkx1 = []
farkp = 0
farkx = 0
oranp = 0              
for k in range(len(maxy)-1):
   farkp = maxy[k] - maxy[k+1]   
   farkp  = abs(farkp)
   farkp1.append(farkp)
   

buyuk1 = 0
kucuk1 = 0
maxy1 = [] #maksimum noktalar arasındaki farkı depolayan dizi
miny1 = [] #minimum noktalar arasındaki farkı depolayan dizi
maxyn1 = []
for t in range(len(maxy)-1): # maxy adli dizinin eleman sayisi kadar dongü baslasin
        t = t + 1
        if t < (len(maxy)-1):
            if maxy[t-1] > maxy[t]: #t-1.eleman t.elemandan büyükise yani azalan 5 4  gibi
                   if maxy[t] > maxy[t+1]: #t.eleman t+1 den büyükse azalmaya devam 4 3 gibi
                         buyuk1 = buyuk1 
                   elif maxy[t] < maxy[t+1]: #t.eleman t+1.eleman dan den kucuk ise orada min var 4 7
                           kucuk1 = maxy[t]
                           miny1.append(kucuk1) #minimum değer miny1 adli değişkene eklenir
                   elif maxy[t] == maxy[t+1]: #t.eleman t+1 e esitse minimum olabilir    4 4 
                           kucuk1 = maxy[t+1]  #aynı sayı olacağından miny1 adli diziye eklemeye gerek yok.             
            elif maxy[t-1] < maxy[t]: #t+1.eleman  t.eleman dan kucuk ise yani artan   3 6
                   if maxy[t] > maxy[t+1]: #t.eleman t+1.eleman dan buyuk ise 6-4 gibi azalmaya baslamıs max olabilir
                             buyuk1 = maxy[t] 
                             maxy1.append(buyuk1) #maksimum nokta diziye eklenir
                             maxyn1.append(t) #maksimum noktanın indeksi eklenir
                   elif maxy[t] < maxy[t+1]: # 2 3den kucuk ise 6 12 
                             kucuk1 = kucuk1
                   elif maxy[t] == maxy[t+1]: #2 3 e esit ise ise    
                             buyuk1 = maxy[t+1]
                   
            elif maxy[t-1] == maxy[t]: #1 2 ye esit ise   5 5    
                   
                 if maxy[t] > maxy[t+1]: #2 de  3 ten  buyuk ise yani azalan 5 3
                         buyuk1 = maxy[t] 
                         maxy1.append(buyuk1)
                         maxyn1.append(t)
                      
                   
                   
                 elif maxy[t] < maxy[t+1]: #2 de 3 den kucuk ise yani artan.. 5 7
                          kucuk1 = maxy[t]
                          miny.append(kucuk)

maxy2 = [] #maksimum noktaların bulunduğu dizideki maksimum noktaların arasındaki farkı depolayacak dizi
farkp2 = 0 #maxy2 nin maksimum noktaları arasındaki farkı depolayacak değişken
n = 0

for k in range(len(maxy1)-1): #
   farkp2 = maxy1[k] - maxy1[k+1]   #maksimum noktaların maksimum noktaları arasındaki fark
   farkp2  = abs(farkp2) #negatif değer olabileceğinden mutlak değeri
   n = n + farkp2 #toplamı
   
   maxy2.append(farkp2)  #diziye eklenmesi
hata = n / (len(maxy2)) #ortalaması
if hata > 3.7:
        print("HATA YÜZDESİ " + " % " + str(hata) + " OLUP RESİM HATALIDIR.")
else:
        print("HATA YÜZDESİ " + " % " + str(hata) + " OLUP RESİM HATALI DEĞİLDİR")
        

plt.plot(maxy2)
plt.xlabel("x")
plt.ylabel("y")
plt.show()

images = [resim,blr1,thresh1,dilation]
title = ['Orjinal','Yumusatma',"Esikleme","Asındırma"]
for i in range(4):
    plt.subplot(2,2,i+1),plt.imshow(images[i],'gray',vmin=0,vmax=255)
    plt.title(title[i]), plt.xticks([]), plt.yticks([])
    
plt.show()
