# Tarefa 1

## APUNTES DDL ##

### INDICE ###
1. [Definición DDL](#id1)
2. [Create](#id2)
	- [Create Database](#id2.1)
	- [Create Domain](#id2.2) 
	- [Create table](#id2.3)
3. [Alter](#id3)
4. [Drop](#id4)
5. [Constraints](#id5)
	- [Constraint primary key](#id5.1)
	- [Constraint foreign key](#id5.2) 
	- [Constraint not null](#id5.3)
	- [Constraint unique](#id5.4)
	- [Constraint check](#id5.5)
6. [Ejemplo creación de una base de datos](#id6)

### DEFINICIÓN DDL<a name="id1"></a> ###

DDL, lenguaje de definición de datos, es un sublenguaje de SQL que permite llevar a cabo las tareas de definición de las estructuras que almacenarán los datos así como de los procedimientos o funciones que permitan consultarlos. Dentro de DDL existen varias sentencias que iremos viendo a continuación.

#### CREATE<a name="id2"></a> ####

La sentencia Create sirve para crear una nueva base de datos ```CREATE DATABASE``` o ```CREATE SCHEMA```, tabla ```CRREATE TABLE```, dominio ```CREATE DOMAIN```, usarios ```CREATE USER```. Create crea un objeto dentro de un sistema de gestión de bases de datos relacionales, podiendo llegar a crear todos los objetos anteriormente puestos. Ahora especificaremos un poco más cada una de estas sentencias.

##### ```CREATE DATABASE ```<a name="id2.1"></a>
Sirve para dar una orden para crear una nueva base de datos, tambien podriamos utilizar la palabra ***SCHEMA*** en vez de ***DATABASE*** para crear una base de datos. La sintaxis que usamos para crear una base de datos es:

	CREATE DATABASE/SCHEMA
	(Si no existe) nombreDATABASE
	(CHARACTER SET nombreCHARSET);

##### `CREATE DOMAIN`<a name ="id2.2"></a>
Sirve para no tener que repetir siempre el mismo dato en una base de datos, así cuando aparezcan diferentes atributos con el mismo tipo de datos usaremos un `CREATE DOMAIN`. La sintaxis que usaremos será:

    CREATE DOMAIN [nombreDATABASE] (nombreDOMINIO) <tipoDato>;
	
##### `CREATE TABLE`<a name ="id2.3"></a>
Sirve para crear una tabla dentro de una base de datos, el contenido que queramos meter dentro de ella tendrá que ir entre paréntesis. Dentro de los paréntesis pueden ir los atributos, los cuales tenemos que especificar los tipos de datos que queremos que vayan con cada atributo, también tendremos que indicar si hay Primary Keys, Foreign o si es NOT NULL , aunque en el segundo y tercer caso no siempre tiene que existir. La sintaxis que se usará será:

	CREATE TABLE [nombreDATABASE] <nombreTABLE> (
		<nombreATRIB1> <tipoDato> {ClavePrimaria}
		<nombreATRIB2> <nombreDOMINIO>...
	);

#### ALTER<a name="id3"></a> ####
Sirve para poder modificar todo tipo de cosas en una base de datos, pero nos centraremos en las tablas `ALTER TABLE`, con el que podemos añadir 
`ADD` o eliminar `DROP` columnas y restricciones. Usando `CASCADE` hacemos que se elimine todo de lo que tenga dependencia una columna o una restricción, usando `RESTRICT` no se eliminará una columna si otras tienen dependencia de ella. La sintaxis que utilizaremos será: 
	
	ALTER TABLE <nombreTABLE>
	ADD <nombreColumna> ...
	DROP <nombreColumna> (CASCADE/RESTRICT);
Otra cosa que podemos utilizar con `ALTER` sería modificar una columna, usaríamos `ALTER COLUMN`

#### DROP<a name="id4"></a> ####
Como ya vimos en la explicación anterior DROP se utiliza para eliminar elementos de una base de datos. La sintaxis que utilizaremos será:

	DROP SCHEMA [NombreDATABASE]
	(CASCADE/RESTRICT);
Podemos usar `CASCADE` si queremos borrar la base de datos aunque tenga datos en ella o podemos usar `RESTRICT` si queremos que no la borre si tiene datos en ella.

Para borrar una tabla seria igual.
	
	DROP TABLE [nombreTABLE]
	(CASCADE/RESTRICT);

#### CONSTRAINT <a name="id5"></a> ####
Sirve para limitar los tipos de datos que podemos meter en una tabla. Estas restricciones podemos añadirlas cuando la creamos con `CREATE TABLE` o después con `ALTER TABLE`. Los tipos de `CONSTRAINT` que veremos serán:

- `PRIMARY KEY`
- `FOREIGN KEY`
- `NOT NULL`
- `UNIQUE`
- `CHECK`

##### CONSTRAINT PRIMARY KEY <a name="id5.1"></a> #####
Asegura que los valores sean únicos para cada registro, cada tabla tiene que tener una obligatoriamente. La sintaxis será:
	
	[CONSTRAINT <nombreCONSTRAINT>]
		PRIMARY KEY (nombreAtributo)
Otra manera de poder usarlo sería:

	CREATE TABLE nombreTABLE(
		nombreAtributo CHAR(*) PRIMARY KEY,
		...
	);
Esto solo lo podemos hacer si la clave principal es un atributo y no varios.

##### CONSTRAINT FOREIGN KEY <a name="id5.2"></a> #####
Sirve para relacionar 2 tablas, cuando la usamos tenemos que indicar a tabla hace referencia esa clave foránea. La sintaxis será:

	FOREIGN KEY (nombreAtributo)
	REFERENCES <nombreTABLE> (nombreAtributoReferenciado)
	[ON DELETE CASCADE / NO ACTION / SET NULL / SET DEFAULT]
	[ON UPDATE CASCADE / NO ACTION / SET NULL / SET DEFAULT];
No es obligatorio poner el nombre del atributo referenciado, si no lo indicamos cogerá la clave principal de la tabla o el atributo que se llame igual que el otro. También podemos ver que tenemos diferentes criterios:

- `ON DELETE`: Sirve para decir que hacer cuando los datos son borrados
- `ON UPDATE`: Sirve para decir que hacer cuando se actualizan los datos.
- `CASCADE`: Sirve para poder eliminar o actualizar los datos que provenga de otra tabla y esten modificados.
- `No ACTION`: Sirve para dejar los datos como estaban, no elimina ni actualiza. Es el valor por defecto si no se indica nada.
- `SET NULL`: Si los datos son modificados o eliminados el valor es nulo.
- `SET DEFAULT`: Si los datos son modificados o eliminados el valor es el predeterminado. Poco usado ya que puede dar problemas.


##### CONSTRAINT NOT NULL <a name="id5.3"></a> #####
Sirve para no aceptar valores nulos. La sintaxis será:
	
	CREATE TABLE nombreTable (
	nombreAtributo1 CHAR(*) PRIMARY KEY
	nombreAtributo2 CHAR(*) NOT NULL,
	...
	);
Esto hace que el atributo 2 no pueda ir vacío, tendremos que darle un valor.

##### CONSTRAINT UNIQUE <a name="id5.4"></a> #####
Sirve para hacer que no se pueda repetir los elementos de la columna, la podemos acompañar junto a `NOT NULL`. La sintaxis será: 
	
	 CREATE TABLE nombreTable (
	nombreAtributo1 CHAR(*) NOT NULL UNIQUE,
	nombreAtributo2 CHAR(*) NOT NULL,
	...
	);
También los podemos utilizar con `CONSTRAINT`:
	
	CONSTRAINT UQ UNIQUE (nombreATRIBUTO,...);

##### CONSTRAINT CHECK <a name="id5.5"></a> #####
Sirve para limitar el rango de valores que puede tener una columna. Una tabla puede tener varios `CHECK`. La sintaxis sería:
	
	 CREATE TABLE nombreTable (
	nombreAtributo1 CHAR(*) PRIMARY KEY,
	nombreAtributo2 CHAR(*) CHECK (valor) ,
	...
	);
También los podemos utilizar con `CONSTRAINT`:

	CONSTRAINT CHECK (nombreATRIBUTO,...)
	CHECK nombreATRIBUTO (valor);

#### Ejemplo creación de una base de datos <a name="id6"></a> ####
![Ejemplo](Img/Ejemplo-1.jpeg)
A través de esta imagen con los datos que nos da el ejercicio haremos la base de datos con lo explicado en estos apuntes.

    CREATE DOMAIN Tipo_Código CHAR(5);
    CREATE DOMAIN Nome_Válido VARCHAR(40);
Primero creamos los domain que vayamos a usar para los atributos, así será más cómodo hacer el ejercicio.
    
    CREATE TABLE Servizo (
      Clave_Servizo Tipo_Codigo,
      Nome_Servizo  Nome_Válido,
      PRIMARY KEY (Clave_Servizo, Nome_Servizo)
    );
Creamos una la tabla Servizo, sus 2 atributos son PK, así que lo indicamos abajo
    
    CREATE TABLE Dependencia (
      Código_Dependencia Tipo_Código,
      Nome_Dependencia   Nome_Válido NOT NULL,
      FunciónVARCHAR(20),
      Localización   VARCHAR(20),
      Clave_Servizo  Tipo_Codigo NOT NULL,
      Nome_Servizo   Nome_Válido NOT NULL,
      PRIMARY KEY (Código_Dependencia),
      UNIQUE (Nome_Dependencia),
      FOREIGN KEY (Clave_Servizo, Nome_Servizo)
    REFERENCES Servizo (Clave_Servizo, Nome_Servizo)
    ON DELETE Cascade
    ON UPDATE Cascade
    );
En la tabla de Dependencia tenemos que añadir como FK los 2 atributos de la tabla anterior. Añadimos un `CONSTRAINT UNIQUE` para Nome_Dependencia para que no se pueda repetir.

    CREATE TABLE Cámara (
      Código_Dependencia Tipo_Código,
      Categoría  Nome_Válido NOT NULL,
      Capacidade INTEGER NOT NULL,
      PRIMARY KEY (Código_Dependencia),
      FOREIGN KEY (Código_Dependencia)
    REFERENCES Dependencia
    ON DELETE Cascade
    ON UPDATE Cascade
    );
En la tabla Cámara añadimos un FK de la tabla Dependencia que es la PK de las dos tablas.

    CREATE TABLE Tripulación (
      Código_Tripulación Tipo_Código PRIMARY KEY,
      Nome_Tripulación   Nome_Válido,
      Categoría  CHAR(20)NOT NULL,
      AntigüidadeINTEGER DEFAULT 0,
      ProcedenciaCHAR(20),
      AdmCHAR(20)NOT NULL,
      Código_Dependencia Tipo_Código NOT NULL,
      Código_Cámara  Tipo_Código NOT NULL,
      FOREIGN KEY (Código_Cámara)
    REFERENCES Cámara (Código_Dependencia)
    ON UPDATE Cascade
    ON DELETE Cascade
    );
En la tabla Tripulación vemos que el atributo de categoría no puede ser nulo, la PK es Código_Tripulación, vemos como hay 2 FK que no pueden ser nulas.
    
    ALTER TABLE Tripulación
      ADD FOREIGN KEY (Código_Dependencia)
    REFERENCES Dependencia (Código_Dependencia)
    ON UPDATE Cascade
    ON DELETE Cascade;
Como falto añadir la FK de Código_Dependencia hacemos un ALTER para agregarla a la base de datos.
    
    CREATE TABLE Planeta (
      Código_Planeta Tipo_Código  PRIMARY KEY,
      Nome_Planeta   Nome_Válido NOT NULL UNIQUE,
      GalaxiaCHAR(15)NOT NULL,
      CoordenadasCHAR(15)NOT NULL,
      UNIQUE(Coordenadas)
     );
Creamos una tabla Planeta con PK Código planeta y no nulos para todos los atributos y las cordenadas unicas ya que no puede haber 2 planetas en el mismo sitio.

    CREATE TABLE Visita (
      Código_Tripulación Tipo_Código,
      Código_Planeta Tipo_Código,
      Data_VisitaDATE,
      Tempo  INTEGER  NOT NULL,
      PRIMARY KEY (Código_Tripulación, Código_Planeta, Data_Visita),
      FOREIGN KEY (Código_Tripulación)
    REFERENCES Tripulación
    ON UPDATE CASCADE
    ON DELETE CASCADE,
      FOREIGN KEY (Código_Planeta)
    REFERENCES Planeta
    ON UPDATE CASCADE
    ON DELETE CASCADE 
    );
La tabla visita tendrá como PK Código_ Tripulación, Código_ Planeta y Data_Visita, Tempo que obligatoriamente será un numero por el Integer y no puede ser nulo, 2 FK, una de la tabla tripulacion (Codigo _Tripulacion) y otra de la tabla Planeta (Código _Planeta). 
    
    CREATE TABLE Habita (
      Código_PlanetaTipo_Código,
      Nome_Raza Nome_Válido,
      Poboación_Parcial INTEGER NOT NULL,
      PRIMARY KEY (Código_Planeta, Nome_Raza),
      FOREIGN KEY (Código_Planeta)
    REFERENCES Planeta
    ON UPDATE Cascade
    ON DELETE Cascade
    );
La tabla Habita tendrá 2 PK una de ellas es FK que pertenece a la tabla Planeta (Código_Planeta), la otra será Nome _Raza. 
    
    CREATE TABLE Raza (
      Nome_Raza   Nome_Válido  PRIMARY KEY,
      Altura  INTEGER  NOT NULL,  -- cm
      Anchura INTEGER  NOT NULL,  -- cm
      Peso INTEGER  NOT NULL,  -- g
      Poboación_Total INTEGER  NOT NULL
    );
La tabla Raza tendrá como PK Nome_Raza y Nome _Valido, los demas atributos no podran ser nulos, los atributos alura, anchura y Peso serán Integers, es decir un número y irán acompañados al final de ellos con su unidad correspondiente.
    
    ALTER TABLE Habita
      ADD CONSTRAINT FK_Raza
    FOREIGN KEY (Nome_Raza)
      REFERENCES Raza
      ON UPDATE CASCADE
      ON DELETE CASCADE;
Al crear la tabla Raza tenemos que añadir una FK a la tabla Habita, el atributo será Nome_Raza.
    
    ALTER TABLE Cámara
      ADD CONSTRAINT Capacidade_maior_de_cero
    CHECK (capacidade > 0);
Tenemos que actualizar la tabla Cámara ya que la  capacidad tiene que ser mayor que 0. 

    CREATE ASSERTION Non_Excedemos_A_Capacidade_Das_Cámaras
    CHECK (NOT EXISTS
      SELECT Cámara.Código_Dependencia
      FROM Cámara
      WHERE Cámara.Capacidade <= (SELECT COUNT (*)
      FROM Tripulación
      GROUP BY Tripulación.Código_Cámara
      HAVING Cámara.Código_Dependencia = Tripulación.Código_Cámara)
    );
Tras establecer una capacidad mínima, tenemos que establecer una capacidad máxima. Esta capacidad máxima sera el total de la tripulación.
    
    CREATE ASSERTION Non_Excedemos_A_Capacidade_Das_Cámaras
      CHECK (
    Cámara.Capacidade <=
      COUNT(*) GROUP BY Tripulación.Código_Cámara);
La capacidad mínima de la cámara será el menor o igual a la tripulación.

