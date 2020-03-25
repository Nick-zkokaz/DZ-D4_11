# Модуль D4
Начальный проект домашней библиотеки.
Для того, чтобы запустить локальный сервер необходимо, например, с помощью терминала PyCharm:
1) Открыть проект в папке C:\my_site
2) Откройте командную строку и зайдите в директорию проекта:
   cd C:\my_site
3) Создать виртуальное окружение через PyCharm Trminal:
   python -m venv django
4) Активировать виртуальное окружение через PyCharm Trminal:
   django\Scripts\activate.bat
5) Установить все необходимые пакеты через PyCharm Trminal:
   pip install -r requirements.txt

6) Выполнить следующий команды через PyCharm Trminal:
   python manage.py makemigrations
   python manage.py migrate
		если не проходят, то с явным указанием приложения:
		python manage.py makemigrations p_library
		python manage.py migrate p_library
   python manage.py loaddata data.xml
			
 
Возможно для использования:   
	python manage.py createsuperuser - возможно не будет пускать в админку, тогда для входа в админку
	Username (leave blank to use '12345'): 12345@mail.ru
    Email address: 12345@mail.ru
    Password:
    Password (again):
    Superuser created successfully.

	python manage.py shell - для пробега по базе как в модуле D3 и D4 смотри команды ниже
7)Запуск сервера
	python manage.py runserver

После запкска сервера - прогон по адресам
Using the URLconf defined in my_site.urls, Django tried these URL patterns, in this order:
http://127.0.0.1:8000/admin/
http://127.0.0.1:8000/index/ -      основной ответ на домашнее задание по этой ссылке
http://127.0.0.1:8000/publishers/
http://127.0.0.1:8000/index/book_increment/
http://127.0.0.1:8000/index/book_decrement/
http://127.0.0.1:8000/ -            пустой попадает в books

Ниже привожу свои записи для прохождения модулей D3 и D4
Они могут понадобиться для "реанимации проекта" с полным повторением команд из модулей D3 и D4
 # новое django-admin startproject my_site
 # python manage.py startapp p_library
 # изменения python manage.py shell # >>> входим в консоль базы данных
 # изменияем INSTALLED_APPS = [ добавляем наше приложение  библиотеку  'p_library']
 # создаем файл миграции python manage.py makemigrations
 # Применим миграции python manage.py migrate
 # откроем консоль Django проекта python manage.py shell
 #>>>from p_library.models import Author
 >>>a = Author(full_name="Николай Васильевич Гоголь", birth_year=1809, country="RU")
 >>>print (a.full_name)
 Николай Васильевич Гоголь
 # удаляем данные файл базы данных db.sqlite3 из директории
 # После удаления этого файла, его нужно снова создать   python manage.py migrate
     может понадобиться запустить Django django\Scripts\activate.bat
 # Итак, база данных готова, фикстуры готовы. Давайте импортируем их:
 # python manage.py loaddata data.xml
 # В консоли вы должны увидеть сообщение Installed 15 object(s) from 1 fixture(s).
 # Откроем shell - python manage.py shell
 # и выведем имена всех авторов
 >>>from p_library.models import Author
 >>>for author in Author.objects.all():
    print(author.full_name)
 Николай Васильевич Гоголь
 Пушкин Александр Сергеевич
 Тургенев Иван Сергеевич
 Douglas Adams
 Jerome David Salinger
 Knut Hamsun
 >>>from p_library.models import Author, Book
 >>>pushkin = Author.objects.get(full_name="Пушкин Александр Сергеевич")
 >>>pushkin_books = Book.objects.filter(author=pushkin)
 >>>pushkin_books.exists()
 True
 >>>pushkin_books.count()
 3
 >>>for book in pushkin_books:
    print(book.title)
 Руслан и Людмила
 Медный всадник
 Граф Нулин
 >>>no_horsman_pushkin_books = Book.objects.all().filter(author=pushkin).exclude(title__icontains="всадник")
 >>>for book in no_horsman_pushkin_books:
    print(book.title)
 Руслан и Людмила
 Граф Нулин
 #####################################
 # При переходе на урок 4 ОБЯЗАТЕЛЬНО заново импортировать Book
 # Для этого в во views добавить импорты
 from django.shortcuts import redirect
 from django.http import HttpResponse
 from django.template import loader
 from p_library.models import Book
