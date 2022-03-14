Install venv

	python -m venv ./venv

2.	Activate venv

	venv/Scripts/activate

3.	Install django

	py -m pip install Django

4.	Create project app

	django-admin startproject ecommerce

5.	Create app

	cd to ecommerce folder and run the following command

	python manage.py startapp store

6. Include store app to settings.py in main project folder
=============================================================

     INSTALLED_APPS = [    
    
        'store',
    ]


To add static folder in settings.py
===================================

    STATIC_URL = 'static/'

    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'static')
    ]

    MEDIA_URL = 'images/' [to add images folder]

In main.html file to load and connect static files
==================================================

    {% load static %}

    <link rel="stylesheet" type="text/css" href="{% static 'css/main.css' %}">

Create superuser => cd to project folder
==================================================

    python manage.py createsuperuser   

Create models in models.py file in project folder
==================================================   

    class Customer(models.Model):
        name = models.CharField(max_length=255, null=True)
        phone = models.CharField(max_length=255, null=True)
        email = models.CharField(max_length=255, null=True)
        date_created = models.DateTimeField(auto_now_add=True, null=True)

        def __str__(self):
            return self.name

    python manage.py  makemigrations
    python manage.py migrate

Create relation to foreign items 
===================================

    class Order(models.Model):
    STATUS = (
        ('Pending', 'Pending'),
        ('Out of delivery', 'Out of delivery'),
        ('Delivered', 'Delivered'),
    )
    customer = models.ForeignKey(Customer, null=True, on_delete=models.SET_NULL)
    product = models.ForeignKey(Product, null=True, on_delete=models.SET_NULL)
    date_created = models.DateTimeField(auto_now_add=True, null=True)
    status = models.CharField(max_length=255, null=True, choices=STATUS)

Register models in admin.py file in project folder
==================================================

    from .models import * [to import all models]
    admin.site.register(Customer)


# Database model queries: https://docs.djangoproject.com/en/4.0/ref/models/querysets/#filter

    filter()
    exclude()
    get()
    create()


Dynamic URL routing and templating
================================================================

    In views.py 
    =============

    def customer(request, pk_test):
    customer = Customer.objects.get(id=pk_test)
    
    orders = customer.order_set.all()

     order_count = orders.count()
    
    context = {'customer': customer, 'orders': orders, 'order_count': order_count}
    
    return render(request, 'products/customer.html', context)


    In urls.python
    ===============

    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.dashboard, name='dashboard'),
        path('customer/<str:pk_test>/', views.customer, name='customer'),
        path('products/', views.products, name='products'),
    ]

    In customer.html
    ====================

    
				{% for order in orders %}

				<tr>
					<td>{{ order.product }}</td>
					<td>{{ order.product.category}}</td>
					<td>{{ order.date_created }}</a></td>
					<td>{{ order.status }}</td>
					<td><a href="">Update</a></td>
					<td><a href="">Remove</a></td>
				</tr>

				{% endfor %}


# CRUD functionality - Tutorial 11(Important)
==============================================

