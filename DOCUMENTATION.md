#PYTHON

##1.1 Variables.

###Nombres.
snake_case para variables y funciones

UPPER_SNAKE_CASE para constantes

Evitar nombres genericos como x, dato.

###Correcto:
  edad_usuario = 20

###Incorrecto:
  EdadUsuario = 20

##1.2 Funciones.

###Documentación:

Usar docstrings en formato Google Style:

def sumatoria(num1,num2):
  '''Realiza la suma de una serie de numeros
    Args:
        num1, num2: Numeros a sumar
    Returns:
        Suma de ambos numeros'''
    return num1+num2

Usar "#" para comentarios de una sola línea:

def sumatoria(num1,num2):
	#Realiza la suma de una serie de numeros
	return num1 + num2	


###Parametros:
Limite maximo de 4 parametros por función. Si hay más, usar **kwargs o clases.

####Correcto:

def conectar_db(host, puerto, usuario, contraseña):
  ...

####Incorrecto:

def conectar_db(host, puerto, usuario, contraseña, timeout, ssl,...)
  ...

##1.3 Clases y Métodos.

###Nombres:

PascalCase para clases.

snake_case para métodos.

class ProcesadorDatos:
	def __init__(self, datos):
		self.datos = datos

	def limpiar(self):
		'''Limpia los datos'''
		...

Métodos especiales:
Documentar **init**, **str** y métodos mágicos.

class Usuario:
	def __init__(self, nombre, edad):
		'''Constructor de la clase Usuario.'''
		self.nombre = nombre
		self.edad = edad

##1.4 Errores Comunes a Evitar:

Mutable Defaults:

####Incorrecto (lista como valor por defecto)

def añadir(valor, inventario = []):
  inventario.append(valor)

####Correcto:

def añadir(valor, inventario):
  inventario = inventario or []
  inventario.append(valor)

##1.5 Modularización de Código:

1. Principios Clave:
	#####Single Responsability Principle (SRP): Cada función/clase debe teenr una única responsabilidad.
	#####DRY (Don't Repeat Yourself): Evitar código duplicado creando funciones reutilizables.
	#####Abstracción: Ocultar detalles complejos detrás de interfaces simples.

2. Estrategias para Dividir Código.
	#####2.1 Dividir funciones largas:

Ejemplo: Antes:
	def process_data(raw_data):
	#Paso 1: Limpiar datos
	limpio = []
	for elemento in raw_data:
		if elemento["value"] > 0:
		  ...

	#Paso 2: Calcular metricas
	total = sum(elemento["value"] for elemento in cleaned)
	...

	#Paso 3: Generar reporte
	report = {
		"total": total,
		"average": avg,
		"items": cleaned
		}
	return report

Ejemplo: Despues:

	def clean_data(raw_data):
		'''Filtra y limpia los datos crudos.'''
		cleaned = []
		for elemento in raw_data:
		if elemento["value"] > 0:
			...

	def calculate_metrics(data):
		'''Calcula total y promedio'''
		total = sum(elemento["value"] for elemento in data)
		...

	def generate_report(cleaned_data):
		'''Combina datos limpios y metricas'''
		metrics = calculate_metrics(cleaned_data)
		...

####Uso
cleaned = clean_data(raw_data)
report = generate_report(cleaned)

2.2 Crear Clases para Entidades Complejas.

Si un bloque maneja multiples propiedades y comportamientos, usa clases.

Ejemplo:

class DataProcessor:
	def __init__(self, raw_data):
		self.raw_data = raw_data
	
	def clean(self):
		'''Limpia los datos''
		self.cleaned_data = [...]

	def calculate_metrics(self):
		'''Calcula metricas basicas'''
		total = sum(elemento["value"] for elemento in self.cleaned_data)
		...

####Uso
processor = DataProcessor(raw_data)
processor.clean()
report = processor.calculate_metrics()

3. Señales de que un Bloque es Demasiado Grande

Longitud: Mas de 30-50 lineas en una funcion.

Anidamiento Profundo: Multiples niveles de if/for/try.

Comentarios Excesivos: Si necesitas explicar cada seccion con comentarios largos.

Dificultad para Hacer Test: Si es complicado escribir pruebas unitarias para el codigo.
