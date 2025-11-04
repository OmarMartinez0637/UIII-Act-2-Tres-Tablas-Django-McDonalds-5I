Perfecto — aquí tienes **los 32 pasos completos** (con todos los comandos, archivos y código necesarios) para crear el proyecto **UIII_McDonalds_0637** con **Django** en **Python**, usando **VS Code**, incluyendo la estructura visual (templates) y el CRUD de **Cliente** sin `forms.py`. He incluido **todo** lo que se necesita copiar/pegar. Al final hay el árbol de carpetas y recordatorio para ejecutar el servidor en el puerto **8026**.

---

# 1 — Crear la carpeta del proyecto

En tu sistema de archivos crea la carpeta principal:

```bash
mkdir UIII_McDonalds_0637
cd UIII_McDonalds_0637
```

---

# 2 — Abrir VS Code sobre la carpeta

Abre VS Code en esa carpeta:

```bash
code .
```

(O desde VS Code: File → Open Folder → seleccionar `UIII_McDonalds_0637`)

---

# 3 — Abrir terminal en VS Code

En VS Code: `Terminal` → `New Terminal`. Se abrirá en la ruta `UIII_McDonalds_0637`.

---

# 4 — Crear entorno virtual `.venv`

En la terminal de VS Code ejecuta:

Windows (PowerShell):

```powershell
python -m venv .venv
```

Linux/macOS:

```bash
python3 -m venv .venv
```

---

# 5 — Activar el entorno virtual

Windows (PowerShell):

```powershell
.venv\Scripts\Activate.ps1
```

Windows (cmd):

```cmd
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

(verás `(.venv)` en el prompt)

---

# 6 — Activar intérprete de Python en VS Code

En VS Code presiona `Ctrl+Shift+P` → escribe `Python: Select Interpreter` → elige `./.venv` (el intérprete dentro de `.venv`).

---

# 7 — Instalar Django

Con el entorno activo:

```bash
pip install django
```

(Para asegurar versión estable, si deseas: `pip install "django>=4.2,<5.0"`)

---

# 8 — Crear proyecto `backend_McDonalds` sin duplicar carpeta

Dentro de `UIII_McDonalds_0637` ejecuta:

```bash
django-admin startproject backend_McDonalds .
```

**Nota:** el punto final (`.`) evita crear una carpeta extra.

Estructura inicial aparecerá: `manage.py` y carpeta `backend_McDonalds/`.

---

# 9 — Ejecutar servidor en el puerto 8026 (prueba)

Aún sin apps, puedes probar:

```bash
python manage.py runserver 8026
```

Luego abre en el navegador:
`http://127.0.0.1:8026/`

(copia y pega el link en el navegador — verás la pantalla de bienvenida de Django si todo está bien).

Detén el servidor con `Ctrl+C`.

---

# 10 — Crear aplicación `app_McDonalds`

Crea la app:

```bash
python manage.py startapp app_McDonalds
```

---

# 11 — Agregar `app_McDonalds` en `settings.py`

Edita `backend_McDonalds/settings.py` y en `INSTALLED_APPS` agrega:

```python
INSTALLED_APPS = [
    # apps por defecto...
    'app_McDonalds',
]
```

Además, si quieres usar plantillas globales, deja `TEMPLATES` como viene por defecto (no es obligatorio cambiarlo).

---

# 12 — Crear estructura de carpetas de templates

Dentro de `app_McDonalds` crea:

```
app_McDonalds/
    templates/
        cliente/
        categoria/
```

Comando (desde la raíz):

```bash
mkdir -p app_McDonalds/templates/cliente
mkdir -p app_McDonalds/templates/categoria
```

---

# 13 — Copiar el modelo `models.py`

Reemplaza o edita `app_McDonalds/models.py` con el contenido que proporcionaste (añado tal cual y con small ajuste de nombres de campo para convenciones si no quieres, pero usaré tal cual):

```python
from django.db import models

# ==========================================
# MODELO: CLIENTE
# ==========================================
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

# ==========================================
# MODELO: PEDIDO
# ==========================================
class Pedido(models.Model):
    id_pedido = models.AutoField(primary_key=True)
    id_cliente = models.ForeignKey(Cliente, on_delete=models.CASCADE, related_name="pedidos")
    id_empleado = models.IntegerField()  # ID de empleado, relacionado con la tabla Empleados
    fecha_pedido = models.DateField(auto_now_add=True)
    total = models.DecimalField(max_digits=10, decimal_places=2)
    estado = models.CharField(max_length=30, choices=[('Pendiente', 'Pendiente'), ('Enviado', 'Enviado'), ('Entregado', 'Entregado')])
    direccion = models.TextField()
    forma_pago = models.CharField(max_length=20, choices=[('Efectivo', 'Efectivo'), ('Tarjeta', 'Tarjeta')])
    
    def __str__(self):
        return f"Pedido #{self.id_pedido} - {self.id_cliente.nombre} {self.id_cliente.apellido}"

# ==========================================
# MODELO: EMPLEADO
# ==========================================
class Empleado(models.Model):
    id_empleado = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=50)
    apellido = models.CharField(max_length=50)
    puesto = models.CharField(max_length=50)
    fecha_ingreso = models.DateField(auto_now_add=True)
    salario = models.DecimalField(max_digits=10, decimal_places=2)
    telefono = models.CharField(max_length=15)
    horario = models.CharField(max_length=20)  # Horario de trabajo (Ejemplo: Mañana, Tarde)
    
    def __str__(self):
        return f"{self.nombre} {self.apellido} - {self.puesto}"
```

---

# 14 — Realizar migraciones (modelos)

Generar y aplicar migraciones:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

# 15 — Registrar modelos en `admin.py`

Edita `app_McDonalds/admin.py`:

```python
from django.contrib import admin
from .models import Cliente, Pedido, Empleado

@admin.register(Cliente)
class ClienteAdmin(admin.ModelAdmin):
    list_display = ('id_cliente', 'nombre', 'apellido', 'email', 'telefono', 'fecha_registro')

@admin.register(Pedido)
class PedidoAdmin(admin.ModelAdmin):
    list_display = ('id_pedido', 'id_cliente', 'id_empleado', 'fecha_pedido', 'total', 'estado')

@admin.register(Empleado)
class EmpleadoAdmin(admin.ModelAdmin):
    list_display = ('id_empleado', 'nombre', 'apellido', 'puesto', 'fecha_ingreso')
```

Luego (opcionalmente) crea superusuario:

```bash
python manage.py createsuperuser
```

Si creaste o cambiaste admin, vuelves a ejecutar `makemigrations` **solo si cambiaste modelos**; no es necesario en admin. Si no cambiaste modelos, no hay migraciones nuevas.

---

# 16 — Crear `urls.py` dentro de `app_McDonalds`

Crea `app_McDonalds/urls.py` con rutas CRUD para clientes:

```python
from django.urls import path
from . import views

app_name = 'app_McDonalds'

urlpatterns = [
    path('', views.inicio_mcdonalds, name='inicio'),
    # Clientes
    path('clientes/agregar/', views.agregar_cliente, name='agregar_cliente'),
    path('clientes/ver/', views.ver_clientes, name='ver_clientes'),
    path('clientes/actualizar/<int:id_cliente>/', views.actualizar_cliente, name='actualizar_cliente'),
    path('clientes/actualizar/realizar/<int:id_cliente>/', views.realizar_actualizacion_cliente, name='realizar_actualizacion_cliente'),
    path('clientes/borrar/<int:id_cliente>/', views.borrar_cliente, name='borrar_cliente'),
]
```

---

# 17 — Conectar `app_McDonalds` a `backend_McDonalds/urls.py`

Edita `backend_McDonalds/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_McDonalds.urls', namespace='app_McDonalds')),
]
```

---

# 18 — Crear vistas en `app_McDonalds/views.py` (CRUD de Cliente)

Reemplaza `views.py` con:

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Cliente
from django.utils import timezone

# Inicio
def inicio_mcdonalds(request):
    # resumen simple: cantidad de clientes
    total_clientes = Cliente.objects.count()
    return render(request, 'inicio.html', {'total_clientes': total_clientes})

# Agregar cliente (muestra formulario y procesa POST)
def agregar_cliente(request):
    if request.method == 'POST':
        nombre = request.POST.get('nombre', '')
        apellido = request.POST.get('apellido', '')
        email = request.POST.get('email', '')
        telefono = request.POST.get('telefono', '')
        direccion = request.POST.get('direccion', '')
        fecha_nacimiento = request.POST.get('fecha_nacimiento', None)

        cliente = Cliente(
            nombre=nombre,
            apellido=apellido,
            email=email,
            telefono=telefono,
            direccion=direccion,
            fecha_nacimiento=fecha_nacimiento
        )
        cliente.save()
        return redirect('app_McDonalds:ver_clientes')

    return render(request, 'cliente/agregar_cliente.html')

# Ver clientes (lista)
def ver_clientes(request):
    clientes = Cliente.objects.all().order_by('-fecha_registro')
    return render(request, 'cliente/ver_clientes.html', {'clientes': clientes})

# Mostrar formulario de actualizar (cargar datos)
def actualizar_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id_cliente=id_cliente)
    return render(request, 'cliente/actualizar_cliente.html', {'cliente': cliente})

# Procesar actualización
def realizar_actualizacion_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id_cliente=id_cliente)
    if request.method == 'POST':
        cliente.nombre = request.POST.get('nombre', cliente.nombre)
        cliente.apellido = request.POST.get('apellido', cliente.apellido)
        cliente.email = request.POST.get('email', cliente.email)
        cliente.telefono = request.POST.get('telefono', cliente.telefono)
        cliente.direccion = request.POST.get('direccion', cliente.direccion)
        fecha_nac = request.POST.get('fecha_nacimiento', None)
        if fecha_nac:
            cliente.fecha_nacimiento = fecha_nac
        cliente.save()
        return redirect('app_McDonalds:ver_clientes')
    return redirect('app_McDonalds:actualizar_cliente', id_cliente=id_cliente)

# Borrar cliente (confirmación y eliminación)
def borrar_cliente(request, id_cliente):
    cliente = get_object_or_404(Cliente, id_cliente=id_cliente)
    if request.method == 'POST':
        cliente.delete()
        return redirect('app_McDonalds:ver_clientes')
    return render(request, 'cliente/borrar_cliente.html', {'cliente': cliente})
```

---

# 19 — Crear `templates/base.html` con Bootstrap

Crea `app_McDonalds/templates/base.html`:

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Sistema de Administración McDonald's</title>
    <!-- Bootstrap CSS CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
      body { padding-top: 70px; background-color: #f8f9fa; }
      footer { position: fixed; bottom: 0; left: 0; right: 0; background: #fff; border-top: 1px solid #eaeaea; padding: 10px 0; }
    </style>
  </head>
  <body>
    {% include 'header.html' %}
    <div class="container mt-4">
      {% block content %}{% endblock %}
    </div>

    {% include 'footer.html' %}

    <!-- Bootstrap JS CDN (bundle incluye Popper) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
  </body>
</html>
```

---

# 20 — `templates/header.html` (incluye navbar)

Crea `app_McDonalds/templates/header.html`:

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light fixed-top shadow-sm">
  <div class="container">
    <a class="navbar-brand" href="{% url 'app_McDonalds:inicio' %}">Sistema de Administración McDonald's</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMain">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navMain">
      {% include 'navbar.html' %}
    </div>
  </div>
</nav>
```

---

# 21 — `templates/navbar.html` (opciones y submenús)

Crea `app_McDonalds/templates/navbar.html`:

```html
<ul class="navbar-nav ms-auto">
  <li class="nav-item"><a class="nav-link" href="{% url 'app_McDonalds:inicio' %}">Inicio</a></li>

  <li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Clientes</a>
    <ul class="dropdown-menu">
      <li><a class="dropdown-item" href="{% url 'app_McDonalds:agregar_cliente' %}">Agregar Cliente</a></li>
      <li><a class="dropdown-item" href="{% url 'app_McDonalds:ver_clientes' %}">Ver Clientes</a></li>
    </ul>
  </li>

  <li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Pedidos</a>
    <ul class="dropdown-menu">
      <li><a class="dropdown-item" href="#">Agregar Pedido</a></li>
      <li><a class="dropdown-item" href="#">Ver Pedidos</a></li>
    </ul>
  </li>

  <li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown">Empleados</a>
    <ul class="dropdown-menu">
      <li><a class="dropdown-item" href="#">Agregar Empleado</a></li>
      <li><a class="dropdown-item" href="#">Ver Empleados</a></li>
    </ul>
  </li>
</ul>
```

---

# 22 — `templates/footer.html`

Crea `app_McDonalds/templates/footer.html`:

```html
<footer class="text-center">
  <div class="container">
    <small>&copy; {{ now|date:"Y" }} Derechos reservados. Creado por Ing. Omar Martínez, Cbtis 128</small>
  </div>
</footer>
```

En `settings.py` añade contexto para `now` si quieres; alternativmente en templates usa `{{ now }}` en el view, pero mantendremos simple: en `inicio_mcdonalds` view puedes agregar `now` si deseas.

---

# 23 — `templates/inicio.html`

Crea `app_McDonalds/templates/inicio.html`:

```html
{% extends 'base.html' %}
{% block content %}
<div class="row">
  <div class="col-md-8">
    <h1>Bienvenido al Sistema de Administración McDonald's</h1>
    <p>Administra clientes, pedidos y empleados de forma sencilla.</p>
    <p>Total de clientes registrados: <strong>{{ total_clientes }}</strong></p>
  </div>
  <div class="col-md-4">
    <img src="https://upload.wikimedia.org/wikipedia/commons/4/4e/McDonald%27s_Golden_Arches.svg" alt="McDonalds" class="img-fluid">
  </div>
</div>
{% endblock %}
```

(La imagen proviene de la red; puedes cambiar URL si prefieres otra.)

---

# 24 — Templates para Cliente — `agregar_cliente.html`

Crea `app_McDonalds/templates/cliente/agregar_cliente.html`:

```html
{% extends 'base.html' %}
{% block content %}
<h2>Agregar Cliente</h2>
<form method="post">
  {% csrf_token %}
  <div class="mb-3">
    <label class="form-label">Nombre</label>
    <input class="form-control" name="nombre" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Apellido</label>
    <input class="form-control" name="apellido" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Email</label>
    <input class="form-control" name="email" type="email" required>
  </div>
  <div class="mb-3">
    <label class="form-label">Teléfono</label>
    <input class="form-control" name="telefono">
  </div>
  <div class="mb-3">
    <label class="form-label">Dirección</label>
    <textarea class="form-control" name="direccion"></textarea>
  </div>
  <div class="mb-3">
    <label class="form-label">Fecha de nacimiento</label>
    <input class="form-control" name="fecha_nacimiento" type="date">
  </div>
  <button class="btn btn-primary" type="submit">Agregar</button>
</form>
{% endblock %}
```

---

# 25 — `ver_clientes.html` (tabla con botones ver, editar, borrar)

Crea `app_McDonalds/templates/cliente/ver_clientes.html`:

```html
{% extends 'base.html' %}
{% block content %}
<h2>Lista de Clientes</h2>
<table class="table table-striped">
  <thead>
    <tr>
      <th>ID</th><th>Nombre</th><th>Email</th><th>Teléfono</th><th>Registro</th><th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    {% for c in clientes %}
    <tr>
      <td>{{ c.id_cliente }}</td>
      <td>{{ c.nombre }} {{ c.apellido }}</td>
      <td>{{ c.email }}</td>
      <td>{{ c.telefono }}</td>
      <td>{{ c.fecha_registro }}</td>
      <td>
        <a class="btn btn-sm btn-info" href="{% url 'app_McDonalds:actualizar_cliente' c.id_cliente %}">Editar</a>
        <a class="btn btn-sm btn-danger" href="{% url 'app_McDonalds:borrar_cliente' c.id_cliente %}">Borrar</a>
      </td>
    </tr>
    {% empty %}
    <tr><td colspan="6">No hay clientes registrados.</td></tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

---

# 26 — `actualizar_cliente.html`

Crea `app_McDonalds/templates/cliente/actualizar_cliente.html`:

```html
{% extends 'base.html' %}
{% block content %}
<h2>Actualizar Cliente</h2>
<form method="post" action="{% url 'app_McDonalds:realizar_actualizacion_cliente' cliente.id_cliente %}">
  {% csrf_token %}
  <div class="mb-3">
    <label class="form-label">Nombre</label>
    <input class="form-control" name="nombre" value="{{ cliente.nombre }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Apellido</label>
    <input class="form-control" name="apellido" value="{{ cliente.apellido }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Email</label>
    <input class="form-control" name="email" value="{{ cliente.email }}" type="email">
  </div>
  <div class="mb-3">
    <label class="form-label">Teléfono</label>
    <input class="form-control" name="telefono" value="{{ cliente.telefono }}">
  </div>
  <div class="mb-3">
    <label class="form-label">Dirección</label>
    <textarea class="form-control" name="direccion">{{ cliente.direccion }}</textarea>
  </div>
  <div class="mb-3">
    <label class="form-label">Fecha de Nacimiento</label>
    <input class="form-control" name="fecha_nacimiento" type="date" value="{{ cliente.fecha_nacimiento }}">
  </div>
  <button class="btn btn-success" type="submit">Guardar cambios</button>
  <a class="btn btn-secondary" href="{% url 'app_McDonalds:ver_clientes' %}">Cancelar</a>
</form>
{% endblock %}
```

---

# 27 — `borrar_cliente.html`

Crea `app_McDonalds/templates/cliente/borrar_cliente.html`:

```html
{% extends 'base.html' %}
{% block content %}
<h2>Confirmar Borrado</h2>
<p>¿Desea eliminar al cliente <strong>{{ cliente.nombre }} {{ cliente.apellido }}</strong>?</p>
<form method="post">
  {% csrf_token %}
  <button class="btn btn-danger" type="submit">Sí, borrar</button>
  <a class="btn btn-secondary" href="{% url 'app_McDonalds:ver_clientes' %}">Cancelar</a>
</form>
{% endblock %}
```

---

# 28 — No usar `forms.py` (ya cumplido)

Todo el formulario se maneja con `request.POST` en las vistas, como solicitaste. No hay `forms.py`.

---

# 29 — Estilos y diseño (colores suaves)

El `base.html` ya incluye estilos básicos y Bootstrap. Para un esquema suave y moderno se usó fondo `#f8f9fa` y tarjetas/tabla de Bootstrap. Si deseas más personalización añade un CSS en `static/` (opcional).

Si quieres agregar `static`, crea `app_McDonalds/static/app_McDonalds/css/custom.css` y enlázalo en `base.html` con `{% load static %}` y `<link href="{% static 'app_McDonalds/css/custom.css' %}" rel="stylesheet">`. (No es obligatorio.)

---

# 30 — Configuración final y revisión de rutas

Asegúrate:

* `backend_McDonalds/settings.py` tiene `app_McDonalds` en `INSTALLED_APPS`.
* `backend_McDonalds/urls.py` incluye `app_McDonalds.urls`.
* `app_McDonalds/templates/` contiene los archivos listados.
* `app_McDonalds/views.py` y `app_McDonalds/urls.py` están guardados.

---

# 31 — Ejecutar servidor en el puerto 8026 (final)

Activa el entorno `.venv` si no está activo y ejecuta:

```bash
python manage.py runserver 8026
```

Abre en el navegador:
`http://127.0.0.1:8026/`

Prueba:

* Inicio (home) — `http://127.0.0.1:8026/`
* Agregar cliente — `http://127.0.0.1:8026/clientes/agregar/`
* Ver clientes — `http://127.0.0.1:8026/clientes/ver/`

---

# 32 — Árbol de carpetas y archivos (estructura completa)

Copia/pega este árbol para tener claro dónde está todo:

```
UIII_McDonalds_0637/
├── .venv/
├── manage.py
├── backend_McDonalds/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── app_McDonalds/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations/
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    ├── views.py
    ├── urls.py
    ├── templates/
    │   ├── base.html
    │   ├── header.html
    │   ├── navbar.html
    │   ├── footer.html
    │   ├── inicio.html
    │   ├── cliente/
    │   │   ├── agregar_cliente.html
    │   │   ├── ver_clientes.html
    │   │   ├── actualizar_cliente.html
    │   │   └── borrar_cliente.html
    │   └── categoria/
    └── static/   (opcional)
        └── app_McDonalds/
            └── css/
                └── custom.css
```

---

## Consejos rápidos / validaciones que pediste explícitamente

* **No se validan entradas**: así lo pediste — los formularios envían lo que haya. (Si se desea en un futuro, agregar validaciones en vistas.)
* **No se usa `forms.py`.**
* **Proyecto funcional**: con los pasos y archivos indicados el CRUD de Cliente funciona (crear, ver, editar, borrar).
* **Estética**: se usó Bootstrap y un esquema suave; puedes modificar `custom.css` si quieres más personalización.
* **Admin**: los modelos están registrados para verlos en `http://127.0.0.1:8026/admin/`.

---

Si quieres, al finalizar puedo:

* Generar un archivo ZIP listo para descargar con la estructura (si quieres que lo cree en mi entorno).
* Añadir validaciones mínimas en vistas o mensajes flash (alerts).
* Extender CRUD para `Pedido` y `Empleado` con interfaces similares.
* Añadir búsqueda/paginación para la lista de clientes.

¿Quieres que haga alguno de esos añadidos ahora?
