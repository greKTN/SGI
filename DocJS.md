### **Apartado inicial**
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `inicio()`  | None | None   |Introduce al usuario al sistema, dando una explicación de las funcionalidades y el objetivo del mismo|
| `obj()`   | None   |None|  Muestra en pantalla el objetivo del sistema, los beneficios que ofrece, su importancia y los distintos problemas que busca solucionar|

### **Sistema de Reportes**
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `fecha()`  | None  | `datetime.now().strftime("%Y-%m-%d %H:%M:%S")`  |Obtiene la fecha en la cual se utilice la funcion, en formato: "Year-Month-Day Hours-Minutes-Seconds", y lo devuelve para su posterior almacenado.|
| `generar_numero_reporte(tipo_reporte)`   |  `tipo_reporte`: Variable la cual almacena el tipo de reporte especificado  |Numero de reporte asignado para la avería especificada.| Revisa la base de datos para comprobar el tipo de reporte, posteriormente verifica que no haya sido almacenado previamente, en cuyo caso, le asigna un numero de reporte único y lo retorna|
| `guardar_reporte(datos, tipo_reporte)`   | `datos`: Contiene la información del reporte.<br><br>`tipo_reporte`: Contiene el tipo especifico de reporte de avería  |None| Comprueba que la información del reporte sea correcta, en cuyo caso la almacena en la base de datos.|
| `Campos comunes()`   |None|Datos relevantes para el reporte de avería, como fecha, hora, descripción, etc.| Pide al usuario toda la información relevanta para el reporte, posteriormente almacenandola y retornandola.|
| `validar_datos(tipo_averia, descripcion, ciudad, direccion)`   |`tipo_averia:` Contiene el tipo específico de avería a resolver. <br><br> `descripcion:` Especificaciones acerca del problema <br><br>  `ciudad:` La localización desde la que se hace el reporte <br><br> `direccion:` El sitio en el cual se encuentra la avería |`True` si todos los campos fueron llenados correctamente; de otra forma, `false`.| Comprueba que todos los campos requeridos para el reporte sean llenados correctamente antes de almacenarlos.|
| `generar_reporte(tipo_reporte, opciones_averia)`   |`tipo_reporte`: Contiene el tipo especifico de reporte de avería <br><br>`opciones_averia`:  Contiene las distintas averias que pueden ocurrir |None.| Get all the data from one user by email.|
| `verifyUser(id)`   |`id`: The unique identifier of the user|`true` if the process was successful; otherwise, `false`.| Get all the data from one user by email.|

- **Folder Structure:**  
  This file is located in the `/utils` directory, which is dedicated to reusable helper functions.

---

