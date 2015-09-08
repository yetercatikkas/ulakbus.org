+++++++++++++++++++++++++++++++
ZEngine ile İş Akışı Temelli Uygulama Geliştirme
+++++++++++++++++++++++++++++++


.. - İş akışı ve iş akışı temelli uygulama.
.. - ZEngine: İş akışı tabanlı web çatısı
..	- Falcon
..	- SpiffWorkflow
..	- Pyoko
..	- Modeller
..	- Ekranlar (Activities)
..	- Görevler (Jobs)
..	- Yetkiler ve Rol tabanlı erişim kontrolü.
.. - Adım adım bir web uygulamasının geliştirilmesi
..	- Geliştirme ortamının kurulumu
..	- Dizin & dosya yapısının oluşturulması
..	- İş akışlarının tasarlanması.
..	- Modellerin tanımlanması.
..	- Ekleme görüntüleme düzenleme ve silme işlemleri için CRUDView kullanımı.
..	- Özelleştirilmiş ekranların oluşturulması.

İş akışı ve iş akışı temelli uygulama
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

İş akışları bir kurumun yürüttüğü işlerin belirli bir notasyona göre görselleştirilmesi amacıyla kullanılırlar. İşlerin kim tarafından, hangi sırayla, hangi koşullara bağlı olarak yürütüleceğinin ilgili personel tarafından üzerinde uzlaşılmış bir standartta ifade edilmesi, iş süreçlerinin kişiler arasında daha kolay ve doğru biçimde anlatılabilmesini sağladığı gibi sürecin iyileştirilmesi için yapılacak değişiklikleri tasarlamayı da kolaylaştırmaktadır.

.. image:: http://pm.zetaops.io/attachments/39/kayit_yenileme_ve_ders_kaydi.png
	:width: 600px

İş akışı tabanlı uygulamalar, belirli bir dilde yazılmış akış diagramlarının, iş akışı motoru (workflow engine) tarafından işletilmesi temeline dayanırlar. İş akış motoru kullanıcı girdileri ve çevre değişkenlerini, iş akışında tanımlanmış koşullara karşı işletir. İlgili koşulun yönlendirdiği adımının (workflow step) etkinleştirilmesi, bu adımla ilişkilendilimiş uygulama metodlarının çalıştırılması ve kullanıcı etkileşimleri arasında akışın durumunun (workflow state) saklanması yine iş akışı motorunun görevidir.


ZEngine Web Çatısı
%%%%%%%%%%%%%%%%%%

ZEngine, Zetaops tarafından Python dili kullanılarak geliştirilen, Ulakbüs projesinin de üzerine inşa edildiği, BPMN 2.0 iş akışlarını destekleyen REST odaklı bir web çatısıdır. Falcon, SpiffWorkflow ve Pyoko olmak üzere üç temel öge üzerine kurulmuş olan ZEngine, iş akışı tabanlı web servislerinin Python nesneleri ile kolayca inşa edilmesini sağlayan, ölçeklenebilir ve güvenli bir platform sunmaktadır.

Falcon
******
Falcon, WSGI standardını destekleyen, REST mimarisinde servisler oluşturmak için geliştirilmiş aşırı hızlı ve hafif bir web çatısıdır.

SpiffWorkflow
*************
Spiffworkflow Python ile yazılmış BPMN 2 notasyonunu destekleyen güçlü bir iş akışı motorudur. ZEngine altında kullanılan sürümüne ihtiyaç duyulan ek işlevlerin eklenmesi ve bakımı Zetaops tarafından yapılmaktadır.

Pyoko
*****
Riak KV için tasarlanmış bir ORM (Object Relational Mapper) aracı olan Pyoko, Riak KV'nin Solr arama motoru ile olan entegrasyonunu tümüyle desteklemekte ve bu iki ürünün tek bir API üzerinden ilişkisel bir veri tabanı rahatlığında kullanılabilmesini olanaklı kılmaktadır.

Modeleler
*********
Uygulamanın konusunu oluşturan varlıklar (entities) Pyoko modelleri olarak tasarlanmakta, bu modeller altında saklanan verilere erişim yine modellerde tanımlanan yetki koşullarına uygun olarak yönetilmektedir. Bir varlıkla doğrudan ilişkili metodlar kendi modelinin altında tanımlanebilmekte, böylece uygulamanın kod organizasyonu kolaylaşmaktadır.

::

	from pyoko.model import Model, field

	class Student(Model):
		name = field.String("Adı", index=True)
		join_date = field.Date("Kayıt tarihi", index=True)