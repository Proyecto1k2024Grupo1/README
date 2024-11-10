# Proyecto Intermodular
# Grupo 1

## Integrantes:
* Jessica Brotons Maciá (https://github.com/LaJessyMiau)
* Silvia Cachón Leiva (https://github.com/39Luka)
* Vanesa Sekeresh (https://github.com/Timia117)

## Sistema de información

Somos una panadería y deseamos crear una base de datos para nuestro negocio.

Necesitamos guardar información de nuestros clientes: nombre completo, dirección, números de teléfono, qué compras ha realizado y en qué fecha.

De los productos queremos saber el tipo (dulce, salado, etc.), si es suministrado por un proveedor o elaborado por nuestros panaderos. Si son propios, además, los ingredientes utilizados y qué proveedor los suministra.

En cada ticket de compra tiene que venir el nombre del producto que se ha comprado, la cantidad, el precio, la fecha, si se le aplicó algún descuento y qué dependiente le atendió. Para realizar las posteriores facturas, también necesitamos saber qué ticket pertenece a cada cliente.

Además, de nuestros servicios a domicilio queremos llevar un control de las entregas donde quede registrado en qué fecha y hora un repartidor ha entregado un pedido a un cliente.

De todos nuestros empleados debemos guardar su DNI, nombre, edad, salario, su puesto de trabajo y si son supervisados por un encargado y cuál es.

## Modelo Entidad-Relación


![Modelo ER Panadería Actualizado](https://github.com/user-attachments/assets/0d594cc3-9119-453a-9438-af149405fd22)



## Justificaciones
El atributo fecha va en compra y no en la relación, porque una compra tiene la fecha del momento en que se realizó y no depende del cliente, ya que esa compra no se vende en diferentes fechas.

En cuanto a los productos, tenemos dos tipos: los suministrados y los que hace la panadería. Como tienen relaciones diferentes, hemos creado una generalización para distinguirlos, la cual, es disjunta porque un producto propio no puede ser ajeno y viceversa. También hemos hecho una generalización en empleados porque comparten ciertos datos, como el DNI, nombre, etc.; pero a su vez hay varios tipos de empleados: panadero, dependiente y repartidor; que desempeñan diferentes funciones y, por tanto, tienen distintas relaciones.

Hemos generado una entidad débil llamada línea de ticket porque un ticket de una compra tiene varias líneas donde se especifica cada uno de los productos que se ha comprado. La cantidad de un producto se incluye en la relación porque la cantidad hace referencia a la entidad de producto y también a la línea de ticket, ya que tenemos cierta cantidad del producto en una línea en específico.

Lo mismo ocurre entre producto propio e ingrediente, ya que un producto propio contiene cierta cantidad de un ingrediente; entre compra y dependiente, ya que una compra la vende un dependiente en una fecha y con un descuento determinado; entre repartidor y compra, ya que un repartidor reparte una compra en una fecha y hora concreta.

En la relación reflexiva de ser empleado en la entidad empleado, hemos utilizado la cardinalidad ninguno o una a ninguno o muchos, porque un empleado no está obligado a ser encargado ni está obligado a tener encargado.



## Dependencias de las tablas

![Orden BD](https://github.com/user-attachments/assets/cab0af55-6317-405e-86fd-b44f1031bb18)

## Modelo Relacional

**PRODUCTO** (<ins>codigo</ins>, nombre, tipo, precio)<br>
PK ->(codigo)<br><br>
**CLIENTE** (<ins>idCliente</ins>, direccion, nombre, apellido1, apellido2)<br>
PK ->(idCliente)<br><br>
**TELEFONO** (<ins>numTelefono</ins>, idCliente*)<br>
PK ->(numTelefono)<br>
FK ->(idCliente) -> CLIENTE<br>
VNN ->(idCliente)<br><br>
**EMPLEADO** (<ins>dni</ins>, salario, fnac, nombre, encargado*)<br>
PK ->(dni)<br>
FK ->(encargado) -> EMPLEADO<br><br>
**PANADERO**(<ins>dni*</ins>)
PK ->(dni)<br>
FK ->(dni) -> EMPLEADO<br><br>
**DEPENDIENTE** (<ins>dni*</ins>, horario)<br>
PK ->(dni)<br>
FK ->(dni) -> EMPLEADO<br><br>
**REPARTIDOR** (<ins>dni*</ins>, matricula)<br>
PK ->(dni)<br>
FK ->(dni) -> EMPLEADO<br><br>
**COMPRA** (<ins>numCompra</ins>, fecha, idCliente*, dniDependiente, descuentoDependiente, fechaDependiente, dniRepartidor*, fechaRepartidor, horaRepartidor)<br>
PK ->(numCompra)<br>
FK ->(idCliente) -> CLIENTE<br>
   ->(dniDependiente) -> DEPENDIENTE<br>
   ->(dniRepartidor) -> REPARTIDOR<br>
VNN ->(idCliente)<br>
    ->(dniDependiente)<br>
    ->(dniRepartidor)<br><br>
**LINEA_DE_TICKET** (<ins>numCompra*</ins>, <ins>numLinea</ins>, codProducto*, cantidad)<br>
PK ->(numCompra, numLinea)<br>
FK ->(numCompra) -> COMPRA<br>
   ->(codProducto) -> PRODUCTO<br>
VNN ->(codProducto)<br><br>
**AJENO** (<ins>codigo*</ins>)<br>
PK ->(codigo)<br>
FK ->(codigo) -> PRODUCTO<br><br>
**PROPIO** (<ins>codigo*</ins>)<br>
PK ->(codigo)<br>
FK ->(codigo) -> PRODUCTO<br><br>
**HACER** (<ins>codProdPropio*</ins>, <ins>dniPanadero*</ins>)<br>
PK ->(codProdPropio, dniPanadero)<br>
FK ->(codProdPropio) -> PROPIO<br>
   ->(dniPanadero) -> PANADERO<br><br>
**PROVEEDOR** (<ins>codProveedor</ins>, nombre)<br>
PK ->(codProveedor)<br><br>
**INGREDIENTE**(<ins>codIngrediente</ins>, nombre, descripción)<br>
PK ->(codIngrediente)<br><br>
**SUMINISTRAR** (<ins>codProdAjeno*</ins>, <ins>codProveedor*</ins>)<br>
PK ->(codProdAjeno, codProveedor)<br>
FK ->(codProdAjeno) ->  AJENO<br>
   ->(codProveedor) -> PROVEEDOR<br><br>
**VENDER** (<ins>codProveedor*</ins>, <ins>codIngrediente*</ins>)<br>
PK ->(codProveedor, codIngrediente)<br>
FK ->(codProveedor) -> PROVEEDOR<br>
   ->(codIngrediente) -> INGREDIENTE<br><br>
**CONTENER** (<ins>codIngrediente*</ins>, <ins>codProdPropio*</ins>, cantidad)<br>
PK ->(codIngrediente, codProdPropio)<br>
FK ->(codIngrediente) -> INGREDIENTE<br>
   ->(codProdPropio) -> PROPIO<br><br>

## Esquema Relacional
![ESQUEMA RELACIONAL PANADERIA](https://github.com/user-attachments/assets/7469af97-0db1-4901-baa0-c8126155a1a4)


