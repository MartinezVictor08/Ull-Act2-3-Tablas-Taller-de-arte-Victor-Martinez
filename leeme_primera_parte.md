Perfecto ✅ Aquí tienes **todo tu documento completo** para pegar en **GitHub** con formato Markdown, comillas correctas, código copiable y los pasos bien organizados para tu proyecto **Taller de Arte** (versión funcional y estructurada igual que el ejemplo de la mueblería):

---

# 🎨 **PROYECTO: TALLER DE ARTE**

**Lenguaje:** Python
**Framework:** Django
**Editor:** Visual Studio Code

---

## 🧩 **PRIMERA PARTE — PROCEDIMIENTO COMPLETO**

---

### 1️⃣ Crear la carpeta del proyecto

1. Abre el **Explorador de archivos**.

2. Crea una nueva carpeta con el nombre:

   ```
   UIII_TallerdeArte_0272
   ```

3. Guarda la carpeta en un lugar fácil de encontrar (por ejemplo en Documentos o Escritorio).

---

### 2️⃣ Abrir VS Code sobre la carpeta

1. Abre **Visual Studio Code**.

2. Selecciona **Archivo → Abrir carpeta**.

3. Busca y selecciona la carpeta:

   ```
   UIII_TallerdeArte_0272
   ```

4. Presiona **Seleccionar carpeta**.

---

### 3️⃣ Abrir la terminal en VS Code

En la barra superior de VS Code:
**Terminal → Nueva terminal**
Se abrirá una terminal en la parte inferior.

---

### 4️⃣ Crear el entorno virtual `.venv` desde la terminal

En la terminal escribe:

```
python -m venv .venv
```

Esto crea una carpeta llamada **.venv** dentro del proyecto.

---

### 5️⃣ Activar el entorno virtual

En la terminal (Windows):

```
.venv\Scripts\activate
```

Si se activa correctamente, verás **(.venv)** al inicio de la línea.

---

### 6️⃣ Activar intérprete de Python en VS Code

1. Presiona **Ctrl + Shift + P**.
2. Escribe **Python: Select Interpreter**.
3. Elige el intérprete:

   ```
   .venv\Scripts\python.exe
   ```

---

### 7️⃣ Instalar Django

Ejecuta:

```
pip install django
```

Verifica la instalación:

```
django-admin --version
```

---

### 8️⃣ Crear el proyecto principal `backend_TallerdeArte`

En la terminal:

```
django-admin startproject backend_TallerdeArte .
```

> El punto **"."** evita que se cree una carpeta duplicada.

---

### 9️⃣ Ejecutar servidor en el puerto 8272

```
python manage.py runserver 8272
```

---

### 🔟 Copiar el enlace y abrir en el navegador

Copia:

```
http://127.0.0.1:8272/
```

y pégalo en tu navegador.
Deberás ver la pantalla de inicio de Django.

---

### 1️⃣1️⃣ Crear la aplicación `app_TallerdeArte`

```
python manage.py startapp app_TallerdeArte
```

---

### 1️⃣2️⃣ Modelo `models.py`

Abre `app_TallerdeArte/models.py` y pega lo siguiente:

```python
from django.db import models

# ==========================================
# MODELO: INSTRUCTOR
# ==========================================
class Instructor(models.Model):
    instructor_id = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    especialidad = models.CharField(max_length=100)
    telefono = models.CharField(max_length=20)
    correo = models.EmailField()
    experiencia_anios = models.PositiveIntegerField()
    sueldo = models.DecimalField(max_digits=8, decimal_places=2)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ==========================================
# MODELO: CURSO
# ==========================================
class Curso(models.Model):
    curso_id = models.AutoField(primary_key=True)
    nombre_curso = models.CharField(max_length=100)
    descripcion = models.TextField()
    salon_num = models.IntegerField()
    duracion_semanas = models.PositiveIntegerField()
    costo = models.DecimalField(max_digits=8, decimal_places=2)
    fecha_inicio = models.DateField()
    cupo_maximo = models.PositiveIntegerField()

    # Relación 1:M (un instructor puede tener muchos cursos)
    instructor = models.ForeignKey(Instructor, on_delete=models.CASCADE, related_name='cursos')

    def __str__(self):
        return self.nombre_curso


# ==========================================
# MODELO: MATERIAL
# ==========================================
class Material(models.Model):
    material_id = models.AutoField(primary_key=True)
    nombre_material = models.CharField(max_length=100)
    tipo = models.CharField(max_length=50)
    marca = models.CharField(max_length=50)
    cantidad_stock = models.PositiveIntegerField()
    costo_unitario = models.DecimalField(max_digits=8, decimal_places=2)
    nombre_proveedor = models.CharField(max_length=100)
    descripcion = models.TextField()

    # Relación 1:M (un curso puede tener muchos materiales)
    curso = models.ForeignKey(Curso, on_delete=models.CASCADE, related_name='materiales')

    def __str__(self):
        return self.nombre_material
```

---

### 1️⃣2️⃣.5️⃣ Realizar las migraciones

```
python manage.py makemigrations
python manage.py migrate
```

---

### 1️⃣3️⃣ Comenzar con el modelo **Instructor**

(Se usará en el CRUD de la primera parte del sistema)

---

### 1️⃣4️⃣ Funciones en `views.py`

Abre `app_TallerdeArte/views.py` y agrega lo siguiente:

```python
from django.shortcuts import render, redirect
from .models import Instructor

# Página de inicio
def inicio_TallerdeArte(request):
    return render(request, 'inicio.html')


# Agregar instructor
def agregar_instructor(request):
    if request.method == 'POST':
        nombre = request.POST['nombre']
        apellido = request.POST['apellido']
        especialidad = request.POST['especialidad']
        telefono = request.POST['telefono']
        correo = request.POST['correo']
        experiencia_anios = request.POST['experiencia_anios']
        sueldo = request.POST['sueldo']

        nuevo = Instructor(
            nombre=nombre,
            apellido=apellido,
            especialidad=especialidad,
            telefono=telefono,
            correo=correo,
            experiencia_anios=experiencia_anios,
            sueldo=sueldo
        )
        nuevo.save()
        return redirect('ver_instructor')
    return render(request, 'instructor/agregar_instructor.html')


# Ver instructores
def ver_instructor(request):
    instructores = Instructor.objects.all()
    return render(request, 'instructor/ver_instructor.html', {'instructores': instructores})


# Actualizar instructor
def actualizar_instructor(request, id):
    instructor = Instructor.objects.get(instructor_id=id)
    return render(request, 'instructor/actualizar_instructor.html', {'instructor': instructor})


# Realizar actualización
def realizar_actualizacion_instructor(request, id):
    instructor = Instructor.objects.get(instructor_id=id)
    if request.method == 'POST':
        instructor.nombre = request.POST['nombre']
        instructor.apellido = request.POST['apellido']
        instructor.especialidad = request.POST['especialidad']
        instructor.telefono = request.POST['telefono']
        instructor.correo = request.POST['correo']
        instructor.experiencia_anios = request.POST['experiencia_anios']
        instructor.sueldo = request.POST['sueldo']
        instructor.save()
        return redirect('ver_instructor')
    return redirect('ver_instructor')


# Borrar instructor
def borrar_instructor(request, id):
    instructor = Instructor.objects.get(instructor_id=id)
    instructor.delete()
    return redirect('ver_instructor')
```

---

### 1️⃣5️⃣ Crear carpeta `templates`

Ruta:

```
app_TallerdeArte/templates/
```

---

### 1️⃣6️⃣ Crear los siguientes archivos:

```
base.html
header.html
navbar.html
footer.html
inicio.html
```

---

### 1️⃣7️⃣ base.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Taller de Arte</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    <main class="container mt-4">
        {% block content %}{% endblock %}
    </main>
    {% include 'footer.html' %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---

### 1️⃣8️⃣ navbar.html

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">🎨 Sistema de Administración Taller de Arte</a>
    <div class="collapse navbar-collapse">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="/">🏠 Inicio</a></li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">👨‍🏫 Instructor</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="/agregar_instructor/">Agregar Instructor</a></li>
            <li><a class="dropdown-item" href="/ver_instructor/">Ver Instructor</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">📚 Curso</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Curso</a></li>
          </ul>
        </li>

        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">🧵 Material</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Material</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

---

### 1️⃣9️⃣ footer.html

```html
<footer class="bg-dark text-white text-center py-3 fixed-bottom">
    <p>© <span id="anio"></span> Creado por Victor Martinez, CBTIS 128</p>
</footer>
<script>
    document.getElementById("anio").innerText = new Date().getFullYear();
</script>
```

---

### 2️⃣0️⃣ inicio.html

```html
{% extends 'base.html' %}
{% block content %}
<div class="text-center">
    <h1 class="mb-3">Bienvenido al Sistema de Administración del Taller de Arte</h1>
    <img src="https://upload.wikimedia.org/wikipedia/commons/4/4c/Art-studio.jpg" width="400">
</div>
{% endblock %}
```

---

### 2️⃣1️⃣ Crear subcarpeta `instructor`

```
app_TallerdeArte/templates/instructor/
```

---

### 2️⃣2️⃣ Archivos dentro de `instructor/`

**agregar_instructor.html**

```html
{% extends 'base.html' %}
{% block content %}
<h2>Agregar Instructor</h2>
<form method="POST">
  {% csrf_token %}
  <input type="text" name="nombre" placeholder="Nombre" class="form-control mb-2">
  <input type="text" name="apellido" placeholder="Apellido" class="form-control mb-2">
  <input type="text" name="especialidad" placeholder="Especialidad" class="form-control mb-2">
  <input type="text" name="telefono" placeholder="Teléfono" class="form-control mb-2">
  <input type="email" name="correo" placeholder="Correo" class="form-control mb-2">
  <input type="number" name="experiencia_anios" placeholder="Años de experiencia" class="form-control mb-2">
  <input type="text" name="sueldo" placeholder="Sueldo" class="form-control mb-2">
  <button type="submit" class="btn btn-success">Guardar</button>
</form>
{% endblock %}
```

**ver_instructor.html**

```html
{% extends 'base.html' %}
{% block content %}
<h2>Lista de Instructores</h2>
<table class="table table-bordered">
  <thead>
    <tr>
      <th>ID</th><th>Nombre</th><th>Apellido</th><th>Especialidad</th><th>Teléfono</th><th>Correo</th><th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    {% for i in instructores %}
    <tr>
      <td>{{ i.instructor_id }}</td>
      <td>{{ i.nombre }}</td>
      <td>{{ i.apellido }}</td>
      <td>{{ i.especialidad }}</td>
      <td>{{ i.telefono }}</td>
      <td>{{ i.correo }}</td>
      <td>
        <a href="/actualizar_instructor/{{ i.instructor_id }}/" class="btn btn-warning btn-sm">Editar</a>
        <a href="/borrar_instructor/{{ i.instructor_id }}/" class="btn btn-danger btn-sm">Borrar</a>
      </td>
    </tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

**actualizar_instructor.html**

```html
{% extends 'base.html' %}
{% block content %}
<h2>Actualizar Instructor</h2>
<form method="POST" action="/realizar_actualizacion_instructor/{{ instructor.instructor_id }}/">
  {% csrf_token %}
  <input type="text" name="nombre" value="{{ instructor.nombre }}" class="form-control mb-2">
  <input type="text" name="apellido" value="{{ instructor.apellido }}" class="form-control mb-2">
  <input type="text" name="especialidad" value="{{ instructor.especialidad }}" class="form-control mb-2">
  <input type="text" name="telefono" value="{{ instructor.telefono }}" class="form-control mb-2">
  <input type="email" name="correo" value="{{ instructor.correo }}" class="form-control mb-2">
  <input type="number" name="experiencia_anios" value="{{ instructor.experiencia_anios }}" class="form-control mb-2">
  <input type="text" name="sueldo" value="{{ instructor.sueldo }}" class="form-control mb-2">
  <button type="submit" class="btn btn-primary">Actualizar</button>
</form>
{% endblock %}
```

---

### 2️⃣4️⃣ urls.py en `app_TallerdeArte`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_TallerdeArte, name='inicio_TallerdeArte'),
    path('agregar_instructor/', views.agregar_instructor, name='agregar_instructor'),
    path('ver_instructor/', views.ver_instructor, name='ver_instructor'),
    path('actualizar_instructor/<int:id>/', views.actualizar_instructor, name='actualizar_instructor'),
    path('realizar_actualizacion_instructor/<int:id>/', views.realizar_actualizacion_instructor, name='realizar_actualizacion_instructor'),
    path('borrar_instructor/<int:id>/', views.borrar_instructor, name='borrar_instructor'),
]
```

---

### 2️⃣5️⃣ Agregar app en `settings.py`

```python
INSTALLED_APPS = [
    ...,
    'app_TallerdeArte',
]
```

---

### 2️⃣6️⃣ Enlazar urls.py del proyecto

Edita `backend_TallerdeArte/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_TallerdeArte.urls')),
]
```

---

### 2️⃣7️⃣ Registrar modelos en `admin.py`

```python
from django.contrib import admin
from .models import Instructor, Curso, Material

admin.site.register(Instructor)
admin.site.register(Curso)
admin.site.register(Material)
```

---

### 2️⃣8️⃣ Estilo

Usa colores **suaves, atractivos y modernos** (ya incluidos con Bootstrap).

---

### 2️⃣9️⃣ Estructura completa de carpetas

```
 UIII_TallerdeArte_0272/
 │
 ├── .venv/
 │
 ├── backend_TallerdeArte/
 │   ├── __init__.py
 │   ├── settings.py
 │   ├── urls.py
 │   ├── asgi.py
 │   └── wsgi.py
 │
 ├── app_TallerdeArte/
 │   ├── __init__.py
 │   ├── admin.py
 │   ├── apps.py
 │   ├── models.py
 │   ├── urls.py
 │   ├── views.py
 │   ├── migrations/
 │   │    └── __init__.py
 │   └── templates/
 │        ├── base.html
 │        ├── header.html
 │        ├── navbar.html
 │        ├── footer.html
 │        ├── inicio.html
 │        └── instructor/
 │             ├── agregar_instructor.html
 │             ├── ver_instructor.html
 │             ├── actualizar_instructor.html
 │             └── borrar_instructor.html
 │
 ├── db.sqlite3
 └── manage.py
```

---

### 3️⃣0️⃣ Ejecutar servidor

```
python manage.py runserver 8272
```

Y entra a:

```
http://127.0.0.1:8272/
```

---

¿Quieres que te lo prepare en un archivo `.md` listo para subir directo a GitHub (con emojis y formato markdown completo)? Puedo generarlo para descargar.
