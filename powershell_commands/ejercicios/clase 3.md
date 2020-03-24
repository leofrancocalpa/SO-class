# Ejercicios Clase 3
1. Cree dos archivos de texto similares (con una o dos líneas diferentes).
   Compárelos empleando ``diff``.

```powershell
PS C:\Users\franco> notepad file1.txt

PS C:\Users\franco> notepad file2.txt

PS C:\Users\franco> diff -ReferenceObject (Get-Content file1.txt) -DifferenceObject (Get-Content file2.txt) -IncludeEqual

InputObject SideIndicator
----------- -------------
linux       ==           
Thread      =>           
Windows     =>           
kernel      <=           
BSD         <=  

```
2. Qué ocurre si se ejecuta:
```powershell
get-service | export-csv servicios.csv | out-file
```
Sale un error: 
```powershell
out-file : Cannot process argument because the value of argument "path" is null. 
Change the value of argument "path" to a non-null value.
```
  
Esto sale porque ``out-file`` necesita especificar el parámetro "path", que es el archivo al cual se le va a enviar la salida.

3. Cómo haría para crear un archivo delimitado por puntos y comas (;)?
   PISTA: Se emplea ``export-csv``, pero con un parámetro adicional.

```powershell
   Get-Process | Export-Csv processes.csv -Delimiter ';'
```

4. ``Export-cliXML`` y ``Export-CSV`` modifican el sistema, porque pueden crear
   y sobreescribir archivos. Existe algún parámetro que evite la
   sobreescritura de un archivo existente? Existe algún parámetro que
   permita que el comando pregunte antes de sobresscribir un archivo?
   
```powershell
Get-Process | Export-Csv processes.csv -NoClobber
```

```powershell
Get-Process | Export-CliXML processes.xml -NoClobber
```
   Se usa el parametro ``-Confirm`` para que pregunte antes de sobreescribir un archivo.

5. Windows emplea configuraciones regionales, lo que incluye el separador de
   listas. En Windows en inglés, el separador de listas es la coma (,).
   Cómo se le dice a ``Export-CSV`` que emplee el separador del sistema en lugar
   de la coma?

Se usa el parametro ``-UseCulture``

```powershell
Get-Process | Export-Csv processes.csv -UseCulture
``` 

6. Identifique un cmdlet que permita generar un número aleatorio.
```powershell
Get-Random
```

7. Identifique un cmdlet que despliegue la fecha y hora actuales.
```powershell
Get-Date
```

8. Qué tipo de objeto produce el cmdlet de la pregunta 7?
  
Produce un objeto ``DateTime``
   
9. Usando el cmdlet de la pregunta 7 y ``select-object``, despliegue solamente
   el día de la semana, así:

```console
   DayOfWeek
   ---------
    Thursday
```
```powershell
   Get-Date | Select-Object DayOfWeek
```

10. Identifique un cmdlet que muestre información acerca de parches (hotfixes)
    instalados en el sistema.
```powershell
   Get-HotFix
```

11. Empleando el cmdlet de la pregunta 10, muestre una lista de parches
    instalados. Luego extienda la expresión para ordenar la lista por fecha
    de instalación, y muestre en pantalla únicamente la fecha de instalación,
    el usuario que instaló el parche, y el ID del parche. Recuerde examinar
    los nombres de las propiedades.
```powershell
    Get-HotFix | Sort-Object -Property InstalledOn | Select-Object -Property InstalledOn, InstalledBy, HotFixID
```

o simplificando un poco 

```powershell
    Get-HotFix | sort InstalledOn | Select-Object InstalledOn,InstalledBy,HotFixID
```

12. Complemente la solución a la pregunta 11, para que el sistema ordene los
    resultados por la descripción del parche, e incluya en el listado la
    descripción, el ID del parche, y la fecha de instalación.
    Escriba los resultados a un archivo HTML.
```powershell
    Get-HotFix | Sort-Object -Property Description | Select-Object -Property Description, HotFixID, InstalledOn | ConvertTo-Html | Out- file parches.html
```

13. Muestre una lista de las 50 entradas más nuevas del log de eventos System.
    Ordene la lista de modo que las entradas más antiguas aparezcan primero;
    las entradas producidas al mismo tiempo deben ordenarse por número índice.
    Muestre el número índice, la hora y la fuente para cada entrada. Escriba
    esta información en un archivo de texto plano.
```powershell
    Get-EventLog system | sort TimeGenerated,Index | Select-Object Index,TimeGenerated,Source -Last 50 > logs.txt
```
