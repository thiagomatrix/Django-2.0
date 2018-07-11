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

###  Views Create

    def create_product(request):
      form = ProductForm(request.POST or None)
      if form.is_valid():
        form.save()
        return redirect(‘list_product')

      return render(request, ‘products.html’, {‘form:form'})


### Views Reader

    def list_product(request):
      products = Product.objects.all() # retorna todas os objetos
      return render(request, ‘products.html’, {‘products:products'})
	
### Views Update

    def update_product(request, id):

      products = Product.objects.get(id=id)

      form = ProductForm(request.POST or None, instance=products)
        if form.is_valid():
              form.save()
              return redirect(‘list_product')

        return render(request, ‘meuApp/up_product.html’, {‘form:form'})

### Views Delete

    def deleta_product(request, id):

      product = Product.objects.get(id=id)

      if request.method == 'POST':
         product.delete()
         return redirect('meuApp_list_product')
      else:
         return render(request, 'meuApp/confirma_deletar.html', {'obj':product, 'url':''})


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
          
## confirma_deletar.html

    <h2>Tem Certeza que deseja Deletar {{obj}}? </h2>

    <form action="" method="POST">

      {% csrf_token %}

      <button type="submit">Confirmar</button>

    </form>

    <a href="/meuApp/{{url}}/">Cancela</a>
    
 ## products.html
 
	<h2>listar Produtos</h2>
	<ul>
		{% for product in products %}
		<li>{{product.desc}}</li>
		<li>{{product.price}}</li>
		<li>{{product.quantity}}</li>
		<li><a href="{% url 'up_product' product.id %}">Alterar</a></li>
		<li><a href="{% url 'del_product' product.id %}">Apagar</a></li>
		<br>
		{% endfor %}
	</ul>

	<form action="{% url 'list_product'  %}" method="post">

		{% csrf_token %}
		{{form.as_p}}

		<button type="submit">Novo Produto</button>

	</form>
	
## Templates

1) crie uma pasta templates dentro do App
2) crie uma pasta com o nome do App dentro de templates
3) crie um arquivo index.html e link em urls

	..
	meuAPP
	--templates
	   --meuApp
	     --index.html
	   
## static

1) crie uma pasta static dentro do App
2) crie uma pasta com o nome do App dentro de templates
3) crie um arquivo index.html e link em urls

	..
	meuAPP
	--templates
	   --meuApp
	     --style.css
	     
## settings.py
	
	DATABASES = {
	    'default': {
		'ENGINE': 'django.db.backends.sqlite3',
		'NAME': os.path.join(BASE_DIR, 'nome_do_banco_db.sqlite3'),
	    }
	}

	STATIC_URL = '/static/'
