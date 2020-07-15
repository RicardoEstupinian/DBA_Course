# Administrador de Base de Datos
![Año](https://img.shields.io/badge/A%C3%B1o-2020-brightgreen)
[![Author](https://img.shields.io/badge/Author-Ricardo%20Estupinian-RED)](https://github.com/RicardoEstupinian)
[![Curso](https://img.shields.io/badge/Curso-Administrado%20de%20Base%20de%20Datos-lightgrey)](https://capacitateparaelempleo.org/pages.php?r=.tema&tagID=4066&load=4066&n=0)
![Sitio](https://img.shields.io/badge/Impartido-Capacitate%20para%20el%20empleo-blue)
<hr>
<p style="text-align:justify;">
Documentación de conceptos y aspectos importantes recopilados del curso Administrador de Bases de Datos de Capacítate para el empleo. Tiene como proposito servir como guia de aprendizaje y consulta de informacion.
</p>
<hr>

## Indice
+ [Data Warehouse](#data-warehouse)
+ [Data Marts](#data-marts)
+ [Esquemas de Data Warehouse](#esquemas-de-data-warehouse)
    + [Esquema de Estrella](#ee)
    + [Esquema de Copo de Nieve](#ecn)
    + [Esquema de Constelacion](#ec)
+ [Funciones en SQL Server](#funciones-en-sql-server)
+ [Ejercicios SQL](#ejercicios-sql)
## Contenido
### Data Warehouse
**Definicion**
<p style="text-align:justify;">
El data warehouse o almacen de datos es una base de datos orientada al análisis de información. El almacen de datos se olvida de las transacciones o procedimientos almacenados en la base de datos y solo se enfoca en extraer y organizar informacion importante almacenada en otras bases de datos.

La funcion del data warehouse es contener datos utiles para el negocio y posteriormente tranformarlos en informacion relevante que se pueda analizar rapidamente, de esta forma los usuarios de negocio pueden realizar consultas o reportes sin tocar o afectar la operacion del sistema transaccional.

</p>

**Caracteristicas del Data Warehouse**
+ Orientado a temas : *Todos los datos deben de estar relacionados respecto al tema a analizar.*
+ Variantes en el tiempo : *Los datos de los cambios en el tiempo deben de quedar registrados para evitar perdidas de informacion*
+ No volatil : *La informacion no se elimina ni edita, unicamente es de lectura*

**Principio de separacion de los datos**
<p style="text-align:justify;">
Para cumplir con la funcion del data warehouse se debe de cumplir con el principio de <strong>"SEPARACION DE LOS DATOS"</strong> que establece que:
</p>

> "Se debe de separar los datos usados en operaciones de la base de datos de los que son guardados en el data warehouse para que nunca coincidan"
<p style="text-align:justify;">
En base al principio anterior se diseñan los ETL, o mejor conocidos como procesos de Extraccion, Transformacion y Carga de datos. Con estos procesos se logra que la consulta de informacion en el almacen no este anclada a la base de datos transaccional.
</p>

+ **Extracion :** *Obtener informacion deseada de datos almacenados.*
+ **Tranformacion :** *Adecuar la informacion a los esquemas del data warehouse.*
+ **Carga :** *Depositar los datos en el data warehouse.*
<hr>

### Data Marts
**Definicion**
<p style="text-align:justify;">
Los data marts son bases de datos departamentales que se alimentan del datawarehouse, con el fin de evitar una busqueda exhaustiva por parte del sistema.
</p>

![DataMart](https://es.talend.com/wp-content/uploads/data-mart-diagram.jpg)
<p style="text-align:justify;">
En cada data mart solo accede un numero limitado de usuarios, que realizan <strong>ANALISIS DE UN PROPOSITO ESPECIFICO</strong> sobre los datos que se obtienen del almacen.
</p>
<hr>

### Esquemas de Data Warehouse
<a id="ee"></a>

+ **Esquema de Estrella** : Es el mas simple de todos los esquemas, se llama estrella porque la tabla de **HECHOS** o la central esta rodeada de las entidades llamadas **DIMENSIONES**.

    ![EE](https://troyanx.com/Hefesto/img_071.png)

    **Caracteristicas** 
    + La tabla de hechos esta rodeada de las tablas dimensiones.
    + La tabla de hechos puede verse como una tabla cruzada en el cula su llave primaria se conforma con las llaves de las tablas dimensiones.
    + Mejora el rendimiento en consulta, ya que solo consulta una tabla (SQL sencillo).
    + Solo hay una tabla de dimension por dimension.
  


<a id="ecn"></a>

+ **Esquema de Copo de Nieve** : Es mas complejo que el esquema de estrella, esta diseñado para el mantenimiento de dimensiones; parecido con un modelo relacional ya que se encuentra en la 3FN.

    ![ECN](https://troyanx.com/Hefesto/img_073.png)

    **Caracteristicas**
    + Mas complejo que el esquema de estrella.
    + Optimiza el espacio en memoria
    + Diseñado para el mantenimineto de dimensiones.
    + Trata de segmentar la informacion extensa.
    + Tablas en 3FN.
    + Las consultas se complican a nivel de SQL por lo tanto posee un bajo rendimiento.

<a id="ec"></a>

+ **Esquema de Constelacion** : Es un esquema mas complejo que los dos anteiores. Existen mas de una tabla de hechos.

    ![EC](https://troyanx.com/Hefesto/img_074.png)

    **Caracteristicas**
    + Existen mas de una tabla de hechos.
    + Genera gran *FLEXIBILIDAD* pero se sacrifica la *FACILIDAD*, complicanto asi la mantenibilidad a futuro.
    + Logra las mismas velocidades de busqueda que el esquema de estrella siempre y cuando se genere una tabla por dimension.
<hr>

### Funciones en SQL Server
+ **Funciones Escalares**: Devuelven un solo valor.

    + Sintaxis SQL Server
    ```sql
    CREATE FUNCTION name_function(
        @p_name_parameter type_parameter,
        @p_name_parameter2 type_parameter
    )
    RETURNS type_return
    BEGIN
        DECLARE @v_variable_name type_variable
        SET @v_variable_name = 20*5
        RETURN @v_variable_name
    END

    ```
    + Sintaxis de llamada a funcion
    ```sql
        SELECT dbo.name_function(param1,param2) FROM table_name
    ```

+ **Funciones en linea**: Son aquellas funciones que regresan un conjunto de resultados correspondiente a un *SELECT* por lo tanto el resultado es una tabla.

    + Sintaxis SQL Server
     ```sql
    CREATE FUNCTION name_function(
        @p_name_parameter type_parameter
    )
    RETURNS table
    BEGIN
        RETURN ( consulta_SQL )
    END

    ```
    + Sintaxis de llamada a funcion
     ```sql
        SELECT * FROM dbo.name_function(param)
    ```
    
+ **Funciones en linea de multiples sentencias**: Se utilizan cuando se requiere mayor logica de proceso.
    + Sintaxis SQL Server
     ```sql
    CREATE FUNCTION name_function(
        @p_name_parameter type_parameter
    )
    RETURNS @v_nombre table (colum1 type_c1, column2 type_c2)
    BEGIN
        INSERT INTO @v_nombre
        SELECT colum1, column2 FROM table_name
        WHERE columx = p_name_parameter
        RETURN
    END
    ```
    + Sintaxis de llamada a funcion
     ```sql
        SELECT * FROM dbo.name_function(param)
    ```
<hr>

### Triggers
<p style="text-align:justify;">
Los triggers  activan procesos automaticos al ejecutar algunas sentencias <strong>DML( INSERT, UPDATE, DELETE )</strong>, cada trigger esta anclado a una tabla.
</p>

+ **TIMING en SQL Server**
    + **FOR**: Se activa primero el trigger y despues la sentencia DML.
    + **AFTER**: Se ejecuta la sentencia DML y despues se ejecuta el trigger.

    + **Sintaxis de Triggers en SQL Server**
     ```sql
    CREATE | ALTER TRIGGER trigger_name
    ON table_name
    [AFTER | FOR ] [INSERT | UPDATE | DELETE]
    AS
    BEGIN
        DECLARE @v_variable type_variable

        -- Asignacion a variable haciendo referencia a los valores insertados en el caso de un trigger INSERT
        SELECT @v_variable = col1 FROM inserted

        -- Ejemplo de UPDATE de un registro en base al trigger
        UPDATE table_name SET colx = @v_variable WHERE ... 
    END
    ```
<hr>

### Ejercicios SQL
>Los ejercicios son en base a la BD restaurada del archivo Northwind.bak. Archivo disponible en el curso.

+ **Funciones**
1. Cree una funcion que reciba el Id de un empleado (Employee) y que regrese el total de ordenes que ha generado.
    ```sql
    CREATE FUNCTION fn_countOrdersByEmployee(@Employee_id int)
    RETURNS int
    BEGIN
        DECLARE @contador int
        SELECT @contador=count(EmployeeID) FROM Orders WHERE @Employee_id= EmployeeID 
        RETURN @contador
    END

    -- Llamada
    SELECT Firstname,dbo.fn_countOrdersByEmployee(EmployeeID) FROM Employees
    ```
2. Crea una funcion que reciva el Id de Orden (Order) y regrese el costo total de ordenes que ha generado.
    ```sql
    CREATE FUNCTION dbo.fn_totalCostByOrder(@Order_id int)
    RETURNS money
    BEGIN
        DECLARE @cost money
        SELECT @cost=SUM(UnitPrice*Quantity) FROM [Order Details] WHERE OrderID=@Order_id;
        RETURN @cost
    END
    
    -- Se muestra el nombre del cliente y el monto total de la orden.
    SELECT DISTINCT c.CompanyName Cliente, dbo.fn_totalCostByOrder(od.OrderID) Monto_Total 
    FROM [Order Details] AS od JOIN Orders AS o 
    ON od.OrderID = o.OrderID
    JOIN Customers AS c
    ON c.CustomerID = o.CustomerID
    ```
3. Crea una funcion que regrese el estado que vendio mas productos.
    ```sql
    ```
4. Crea una funcion que muestre el registro del empleado que haya sacado mas dinero por sus ordenes de venta.
    ```sql
    ```
5. Crea unafuncion que recibaun Id del cliente (Customer) y regrese todas las ordenes que ha hecho.
    ```sql
    ```
6. Crea una funcion que reciba un Id empleado y regrese los registros de clientes que ha atendido.
    ```sql
    ```
+ **Triggers**
1. Crea un trigger que se dispare al insertar una nueva orden y haga un Update a la tabla empleados al cambo N_Productos donde lleve el total de productos vendidos.
    >**Nota**: Primero se debe de agregar un nuevo campo N_Productos de tipo INT en la tabla empleados.
    ```sql
    ```

<hr>

[Indice](#indice)

[Regresar al Inicio](#administrador-de-base-de-datos)
