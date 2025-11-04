Claro, aquí te dejo **todos los 32 pasos** del proyecto **McDonald's en Django**, junto con la **estructura visual** completa, para que tengas todo en un solo lugar.

---

### **32 Pasos Completos para el Proyecto McDonald's en Django**

---

#### **1. Procedimiento para crear carpeta del Proyecto: UIII_McDonalds_0637**

1. Abre la terminal.
2. Crea la carpeta:

   ```bash
   mkdir UIII_McDonalds_0637
   ```

#### **2. Procedimiento para abrir VS Code sobre la carpeta UIII_McDonalds_0637**

1. Entra en la carpeta creada:

   ```bash
   cd UIII_McDonalds_0637
   ```
2. Abre Visual Studio Code:

   ```bash
   code .
   ```

#### **3. Procedimiento para abrir terminal en VS Code**

1. En VS Code, ve a "Ver" > "Terminal" o usa el atajo `Ctrl + '`.

#### **4. Procedimiento para crear carpeta entorno virtual “.venv” desde terminal de VS Code**

1. En la terminal, crea el entorno virtual:

   ```bash
   python -m venv .venv
   ```

#### **5. Procedimiento para activar el entorno virtual**

1. **En Windows:**

   ```bash
   .venv\Scripts\activate
   ```
2. **En macOS/Linux:**

   ```bash
   source .venv/bin/activate
   ```

#### **6. Procedimiento para activar intérprete de Python**

1. Selecciona el intérprete de Python desde VS Code (Comando Paleta: `Ctrl + Shift + P` > `Python: Select Interpreter`).
2. Escoge el intérprete dentro de `.venv`.

---

### **Instalación de Django y Creación del Proyecto**

#### **7. Procedimiento para instalar Django**

1. Instala Django en el entorno virtual:

   ```bash
   pip install django
   ```

#### **8. Procedimiento para crear el proyecto backend_McDonalds sin duplicar carpeta**

1. Crea el proyecto Django:

   ```bash
   django-admin startproject backend_McDonalds
   ```

#### **9. Procedimiento para ejecutar servidor en el puerto 8026**

1. Navega al directorio del proyecto:

   ```bash
   cd backend_McDonalds
   ```
2. Ejecuta el servidor en el puerto 8026:

   ```bash
   python manage.py runserver 8026
   ```

#### **10. Procedimiento para copiar y pegar el link en el navegador**

1. Copia el link del servidor (`http://127.0.0.1:8026/`) y pégalo en tu navegador.

---

### **Creación de la Aplicación (app_McDonalds)**

#### **11. Procedimiento para crear la aplicación app_McDonalds**

1. Crea la aplicación:

   ```bash
   python manage.py startapp app_McDonalds
   ```

#### **12. Configuración de la aplicación en `settings.py`**

1. En `backend_McDonalds/settings.py`, agrega `'app_McDonalds'` en `INSTALLED_APPS`:

   ```python
   INSTALLED_APPS = [
       ...
       'app_McDonalds',
       ...
   ]
   ```

---

### **Creación de Modelos**

#### **13. Modelo Cliente (models.py)**

1. Crea el modelo `Cliente` en `app_McDonalds/models.py`:

   ```python
   from django.db import models

   class Cliente(models.Model):
       id_cliente = models.AutoField(primary_key=True)
       nombre = models.CharField(max_length=50)
       apellido = models.CharField(max_length=50)
       email = models.EmailField(unique=True)
       telefono = models.CharField(max_length=15)
       direccion = models.TextField()
       fecha_registro = models.DateField(auto_now_add=True)
       fecha_nacimiento = models.DateField()

       def __str__(self):
           return f"{self.nombre} {self.apellido}"
   ```

#### **14. Modelo Pedido (models.py)**

1. Crea el modelo `Pedido` en `app_McDonalds/models.py`:

   ```python
   class Pedido(models.Model):
       id_pedido = models.AutoField(primary_key=True)
       id_cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name="pedidos")
       id_empleado = models.IntegerField()
       fecha_pedido = models.DateField(auto_now_add=True)
       total = models.DecimalField(max_digits=10, decimal_places=2)
       estado = models.CharField(max_length=30, choices=[('Pendiente', 'Pendiente'), ('Enviado', 'Enviado'), ('Entregado', 'Entregado')])
       direccion = models.TextField()
       forma_pago = models.CharField(max_length=20, choices=[('Efectivo', 'Efectivo'), ('Tarjeta', 'Tarjeta')])

       def __str__(self):
           return f"Pedido #{self.id_pedido} - {self.id_cliente.nombre} {self.id_cliente.apellido}"
   ```

#### **15. Modelo Empleado (models.py)**

1. Crea el modelo `Empleado` en `app_McDonalds/models.py`:

   ```python
   class Empleado(models.Model):
       id_empleado = models.AutoField(primary_key=True)
       nombre = models.CharField(max_length=50)
       apellido = models.CharField(max_length=50)
       puesto = models.CharField(max_length=50)
       fecha_ingreso = models.DateField(auto_now_add=True)
       salario = models.DecimalField(max_digits=10, decimal_places=2)
       telefono = models.CharField(max_length=15)
       horario = models.CharField(max_length=20)

       def __str__(self):
           return f"{self.nombre} {self.apellido} - {self.puesto}"
   ```

#### **16. Procedimiento para realizar las migraciones**

1. Genera las migraciones:

   ```bash
   python manage.py makemigrations
   ```
2. Aplica las migraciones:

   ```bash
   python manage.py migrate
   ```

---

### **Creación de Vistas (views.py)**

#### **17. Procedimiento para trabajar con el modelo CLIENTE en vistas**

1. Crea las vistas de CRUD para `Cliente` en `app_McDonalds/views.py`:

   ```python
   from django.shortcuts import render, redirect
   from .models import Cliente

   def inicio_mcdonalds(request):
       return render(request, 'inicio.html')

   def agregar_cliente(request):
       if request.method == 'POST':
           cliente = Cliente(
               nombre=request.POST['nombre'],
               apellido=request.POST['apellido'],
               email=request.POST['email'],
               telefono=request.POST['telefono'],
               direccion=request.POST['direccion'],
               fecha_nacimiento=request.POST['fecha_nacimiento']
           )
           cliente.save()
           return redirect('ver_clientes')
       return render(request, 'agregar_cliente.html')

   def actualizar_cliente(request, id_cliente):
       cliente = Cliente.objects.get(id_cliente=id_cliente)
       if request.method == 'POST':
           cliente.nombre = request.POST['nombre']
           cliente.apellido = request.POST['apellido']
           cliente.email = request.POST['email']
           cliente.telefono = request.POST['telefono']
           cliente.direccion = request.POST['direccion']
           cliente.fecha_nacimiento = request.POST['fecha_nacimiento']
           cliente.save()
           return redirect('ver_clientes')
       return render(request, 'actualizar_cliente.html', {'cliente': cliente})

   def borrar_cliente(request, id_cliente):
       cliente = Cliente.objects.get(id_cliente=id_cliente)
       if request.method == 'POST':
           cliente.delete()
           return redirect('ver_clientes')
       return render(request, 'borrar_cliente.html', {'cliente': cliente})

   def ver_clientes(request):
       clientes = Cliente.objects.all()
       return render(request, 'ver_clientes.html', {'clientes': clientes})
   ```

---

### **Plantillas HTML**

#### **18. Procedimiento para crear la carpeta “templates” dentro de app_McDonalds**

1. Crea la carpeta `templates` dentro de `app_McDonalds`:

   ```bash
   mkdir -p app_McDonalds/templates
   ```

#### **19. Procedimiento para crear archivos HTML básicos**

1. Crea los archivos base: `base.html`, `header.html`, `navbar.html`, `footer.html`, `inicio.html`.

2. En `base.html`, agrega el siguiente código para incluir Bootstrap:

   ```html
   <!DOCTYPE html>
   <html lang="es">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>McDonald's - Sistema de Administración</title>
       <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
       <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
       <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
   </head>
   <body>
       {% include 'header.html' %}
       {% include 'navbar.html' %}
       <div class="container">
           {% block content %}
           {% end
   ```


block %} </div>
{% include 'footer.html' %} </body> </html>
```

#### **20. Procedimiento para crear archivo navbar.html**

1. En `navbar.html`:

   ```html
   <nav class="navbar navbar-expand-lg navbar-light bg-light">
       <a class="navbar-brand" href="#">McDonald's</a>
       <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
           <span class="navbar-toggler-icon"></span>
       </button>
       <div class="collapse navbar-collapse" id="navbarNav">
           <ul class="navbar-nav">
               <li class="nav-item active">
                   <a class="nav-link" href="#">Inicio</a>
               </li>
               <li class="nav-item dropdown">
                   <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                       Clientes
                   </a>
                   <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                       <a class="dropdown-item" href="#">Agregar Cliente</a>
                       <a class="dropdown-item" href="#">Ver Clientes</a>
                       <a class="dropdown-item" href="#">Actualizar Cliente</a>
                       <a class="dropdown-item" href="#">Borrar Cliente</a>
                   </div>
               </li>
           </ul>
       </div>
   </nav>
   ```

#### **21. Procedimiento para crear archivo footer.html**

1. En `footer.html`:

   ```html
   <footer class="bg-light text-center py-3 fixed-bottom">
       <p>&copy; {{ now.year }} Creado por Ing. Omar Martínez, Cbtis 128</p>
   </footer>
   ```

---

### **Rutas y Configuración Final**

#### **22. Procedimiento para crear archivo urls.py en app_McDonalds**

1. Crea las rutas para las vistas CRUD en `app_McDonalds/urls.py`:

   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('', views.inicio_mcdonalds, name='inicio_mcdonalds'),
       path('agregar_cliente/', views.agregar_cliente, name='agregar_cliente'),
       path('ver_clientes/', views.ver_clientes, name='ver_clientes'),
       path('actualizar_cliente/<int:id_cliente>/', views.actualizar_cliente, name='actualizar_cliente'),
       path('borrar_cliente/<int:id_cliente>/', views.borrar_cliente, name='borrar_cliente'),
   ]
   ```

#### **23. Procedimiento para configurar URLs en `backend_McDonalds`**

1. En `backend_McDonalds/urls.py`, agrega el siguiente código:

   ```python
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('', include('app_McDonalds.urls')),
   ]
   ```

---

### **Finalización**

#### **24. Procedimiento para registrar los modelos en admin.py**

1. Registra los modelos en `app_McDonalds/admin.py`:

   ```python
   from django.contrib import admin
   from .models import Cliente, Pedido, Empleado

   admin.site.register(Cliente)
   admin.site.register(Pedido)
   admin.site.register(Empleado)
   ```

#### **25. Procedimiento para realizar las migraciones nuevamente**

1. Genera las migraciones:

   ```bash
   python manage.py makemigrations
   ```
2. Aplica las migraciones:

   ```bash
   python manage.py migrate
   ```

#### **26-32. Procedimientos de diseño y ejecución final**

1. **Colores suaves y modernos:** Utiliza paletas de colores atractivas (como tonos pasteles) y componentes de **Bootstrap** para un diseño limpio y moderno.
2. **Validación de entrada:** Aunque no necesitas validar la entrada de datos, sería bueno tener en cuenta que la validación de formularios es importante para la seguridad.
3. **Ejecución final:** Una vez completados todos los pasos, ejecuta el servidor para probar que todo funcione correctamente:

   ```bash
   python manage.py runserver 8026
   ```

---

### **Estructura Visual del Proyecto**

```
UIII_McDonalds_0637/
├── backend_McDonalds/                          # Carpeta principal del proyecto Django
│   ├── backend_McDonalds/                      # Carpeta del proyecto Django (contiene configuraciones principales)
│   │   ├── __init__.py
│   │   ├── settings.py                         # Configuración del proyecto Django (base de datos, apps, etc.)
│   │   ├── urls.py                             # Configuración de URLs globales del proyecto
│   │   ├── wsgi.py                             # Configuración WSGI para el servidor
│   │   └── asgi.py                             # Configuración ASGI para el servidor
│   ├── app_McDonalds/                          # Carpeta de la aplicación Django donde se encuentra la lógica
│   │   ├── __init__.py
│   │   ├── admin.py                            # Registro de modelos en el panel admin de Django
│   │   ├── apps.py                             # Configuración de la aplicación
│   │   ├── migrations/                         # Carpeta para las migraciones de la base de datos
│   │   │   └── __init__.py
│   │   ├── models.py                           # Definición de los modelos de Cliente, Pedido, Empleado
│   │   ├── tests.py                            # Pruebas unitarias
│   │   ├── views.py                            # Vistas para manejar el CRUD de los modelos
│   │   ├── urls.py                             # URLs específicas de la aplicación
│   │   └── templates/                          # Carpeta para los archivos HTML de la aplicación
│   │       ├── base.html                       # Plantilla base de las páginas
│   │       ├── footer.html                     # Plantilla para el pie de página
│   │       ├── header.html                     # Cabecera de las páginas
│   │       ├── navbar.html                     # Barra de navegación
│   │       ├── inicio.html                     # Página principal del sistema
│   │       └── cliente/                        # Subcarpeta para templates de clientes
│   │           ├── agregar_cliente.html        # Vista para agregar un cliente
│   │           ├── actualizar_cliente.html     # Vista para actualizar un cliente
│   │           ├── borrar_cliente.html         # Vista para borrar un cliente
│   │           └── ver_clientes.html           # Vista para ver la lista de clientes
│   ├── manage.py                               # Script principal para administrar el proyecto Django
├── .venv/                                      # Carpeta del entorno virtual
│   ├── Scripts/                                # Archivos ejecutables para el entorno virtual
│   ├── Lib/                                    # Librerías instaladas dentro del entorno virtual
│   └── Include/                                # Archivos de inclusión del entorno virtual
├── db.sqlite3                                  # Base de datos SQLite (si se usa SQLite como motor)
└── requirements.txt                            # Archivo de dependencias del proyecto (Django, etc.)

```

---

Con estos pasos y la estructura visual, deberías tener todo lo necesario para desarrollar y ejecutar tu sistema de administración para McDonald's.
