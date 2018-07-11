# Django-2.0

Django 2.0

##  Criar a vens e start vent (activate)

    virtualenv -p python3 .venv

### Windows

    cd vens
    activate

### MacOS

    vens/bin/activate

## Criar um projeto

    django django-admin startproject nome .

## Cria um App

    django django-admin startapp nome

##  Registrar a app em settings

    INSTALLED_APPS = [ “meuApp” ]

##  Em Urls adiciona include após a path

    from django.urls import path, include

##  Fazer include das Urls do projeto

    path(‘meuApp/', include(‘meuApp.urls')),

## Criar a Urls do app - em meuApp/urls.py

    from django.urls import path
    from .views import ….

    urlpatterns = [
        path('', home, name=‘meuApp_home'),

## CRUD - urls.py

    from django.urls import path
    from .views import list_product, create_product…

    urlpatterns = [

    path(‘’, list_product, name=‘list_product’),
    path(’new’, list_product, name=‘list_product’),
    path(‘update/<int:id>’, up_product, name=‘up_product’),
    path(‘delete/<int:id>’, del_product, name=‘del_product’)

    ]

## Views.py

    from django.shortcuts import render, redirect
    from .models import Product
    from .forms import ProductForm

### Create

    def create_product(request):
      form = ProductForm(request.POST or None)
      if form.is_valid():
        form.save()
        return redirect(‘list_product')

      return render(request, ‘products.html’, {‘form:form'})


### Reader

    def list_product(request):
      products = Product.objects.all() # retorna todas os objetos
      return render(request, ‘products.html’, {‘products:products'})
	
### Update

    def update_product(request, id):

      products = Product.objects.get(id=id)

      form = ProductForm(request.POST or None, instance=products)
        if form.is_valid():
              form.save()
              return redirect(‘list_product')

        return render(request, ‘meuApp/up_product.html’, {‘form:form'})

### Delete

    def deleta_product(request, id):

      product = Product.objects.get(id=id)

      if request.method == 'POST':
         product.delete()
         return redirect('meuApp_list_product')
      else:
         return render(request, 'meuApp/confirma_deletar.html', {'obj':product, 'url':'list_product'})


## Models.py

    from django.db import models

    class Product(models.Model):
      desc = models.CharField(max_length=100)
      price = models.DecimalField(max_digits=9, decimal_places=2)
      quantity = models.IntegerField)

      def __str__(self):

        return self.desc

## Forms.py

    from django.forms import ModelForm
    from .models import Product

    class ProductForm(ModelForm):
        class Meta:
            model = Product
            fields = '__all__'
          
## Cornfirma Deletar

    <h2>Tem Certeza que deseja Deletar {{obj}}? </h2>

    <form action="" method="POST">

      {% csrf_token %}

      <button type="submit">Confirmar</button>

    </form>

    <a href="/meuApp/{{url}}/">Cancela</a>
