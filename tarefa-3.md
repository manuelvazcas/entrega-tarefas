# Tarefa 3

## Creación de las base de datos en MariaDB ##
Tras hacer los 2 ejercicios los pasaremos a MariaDB usando la ventana de comandos. 

### INDICE ###
- [Ejercicio 1](#ej1)
	- [creación base de datos](#CBDD)
	- [CREATE TABLES](#CT)
	- [ALTER TABLES](#AT)
- [Ejercicio 2](#ej2)
	- [creación base de datos](#CBDD2)
	- [CREATE TABLES](#CT2)
	- [ALTER TABLES](#AT2)

### Ejercicio 1 <a name="ej1"></a> ###
Antes  de empezar el ejercicio hay que especificar un poco los criterios que seguiremos para hacer la base de datos.

- Prohibido usar acentos y espacios. Usaremos _ para como espacio.
- No se pueden crear dominios.
- Añadiremos `ALTER` al final del ejercicio por si se nos olvidar añadir algún dato.

#### Creación Base de datos <a name="CBDD"></a> ####
Abrimos la ventana de comandos y entramos en MariaDB con el comando `mysql -u root -p` y creamos la base de datos `CREATE DATABASE proyecto_investigacion` y escribimos `use proyecto_investigacion` para usar esa base de datos para el ejercicio.![](Img/ejercicio1-1.PNG)

#### Creación tablas <a name="CT"></a> ####
Empezamos a crear las tablas.

##### Sede #####
	CREATE TABLE Sede (
    	Nombre_Sede VARCHAR(50) PRIMARY KEY,
    	Campus      VARCHAR(50) NOT NULL
	);
En MariaDB:

![](Img/ejercicio1-3.PNG)

##### Departamento #####
	CREATE TABLE Departamento (
    	Nombre_Departamento VARCHAR(50) PRIMARY KEY,
    	Teléfono            VARCHAR(9)
	);
En MariaDB:

![](Img/ejercicio1-2.PNG)

##### Ubicación #####

	 CREATE TABLE Ubicación (
    	Nombre_Departamento VARCHAR(50),
    	Nombre_Sede         VARCHAR(50),
    	CONSTRAINT PK_Ubicacion PRIMARY KEY (Nombre_Departamento, Nombre_Sede),
    	FOREIGN KEY (Nombre_Departamento) REFERENCES Departamento(Nombre_Departamento) 
    	ON UPDATE CASCADE 
    	ON DELETE CASCADE,
    	FOREIGN KEY (Nombre_Sede) REFERENCES Sede (Nombre_Sede) 
    	ON UPDATE CASCADE 
    	ON DELETE CASCADE
	);
En MariaDB:

![](Img/ejercicio1-4.PNG)

##### Grupo investigación #####

	CREATE TABLE Grupo_Investigación (
    	Nombre_Grupo        VARCHAR(50),
    	Nombre_Departamento VARCHAR(50),
    	Area_Conocimiento   VARCHAR(50) NOT NULL,
    	CONSTRAINT PK_Grupo_Investigacion PRIMARY KEY (Nombre_Grupo, Nombre_Departamento),
    	FOREIGN KEY (Nombre_Departamento) REFERENCES Departamento(Nombre_Departamento) 
	    ON UPDATE CASCADE 
	    ON DELETE CASCADE
	);
En MariaDB:

![](Img/ejercicio1-5.PNG)

##### Profesor #####

	 CREATE TABLE Profesor (
    	DNI            CHAR(9) PRIMARY KEY,
    	Nombre         VARCHAR(50) NOT NULL,
    	Titulación     VARCHAR(50) NOT NULL,
    	Experiencia    INTEGER,
    	N_Grupo        VARCHAR(50),
    	N_Departamento VARCHAR(50),
    	CONSTRAINT FK_Grupo_Profesor FOREIGN KEY (N_Grupo, N_Departamento) REFERENCES Grupo_Investigacion(Nombre_Grupo, Nombre_Departamento)
    	ON UPDATE CASCADE 
    	ON DELETE SET NULL  
	);	
En MariaDB:

![](Img/ejercicio1-6.PNG)

##### Participación #####
Pararemos de escribir el CONSTRAINT entero.

	 CREATE TABLE Participacion (
    	DNI_Profesor CHAR(9),
    	Codigo_Proyecto CHAR(5),
    	Fecha_Incorporación DATE,
    	Fecha_Cese DATE,
    	Horas_Semanales INTEGER NOT NULL,
    	PRIMARY KEY (DNI_Profesor, Codigo_Proyecto),
    	FOREIGN KEY (DNI_Profesor) REFERENCES Profesor(DNI) 
    	ON UPDATE CASCADE
    	ON DELETE NO ACTION 
	);
En MariaDB:

![](Img/ejercicio1-7.PNG)

##### Proyecto #####

	CREATE TABLE Proyecto_Investigacion (
    	Codigo_Proyecto CHAR(5) PRIMARY KEY, 
    	Nombre_Proyecto VARCHAR(50) NOT NULL,
    	Presupuesto INTEGER,
    	Fecha_Inicio DATE,
    	Fecha_Fin    DATE,
    	N_Grupo VARCHAR(50),
    	N_Departamento VARCHAR(50),
    	FOREIGN KEY (N_Grupo, N_Departamento) REFERENCES Grupo_Investigacion(Nombre_Grupo, nombre_Departamento)
    	ON UPDATE CASCADE 
    	ON DELETE NO ACTION
	);
En MariaDB:

![](Img/ejercicio1-8.PNG)

##### Financiación #####

	CREATE TABLE Financiacion (
    	Codigo_Proyecto VARCHAR(5),
    	Nombre_Programa VARCHAR(50),
    	Numero_Asociado INTEGER NOT NULL,
    	Cantidad_Dinero INTEGER NOT NULL,  
    	PRIMARY KEY (Codigo_Proyecto, Nombre_Programa),
    	FOREIGN KEY (Codigo_Proyecto) REFERENCES Proyecto_Investigacion(Codigo_Proyecto) 
    	ON UPDATE CASCADE
    	ON DELETE CASCADE
	);
En `Cantidad_Dinero` pongo INTEGER ya que MONEY en MariaDB no funciona.

En MariaDB:

![](Img/ejercicio1-9.PNG)

##### Programa #####

	CREATE TABLE Programa (
	Nombre_Programa VARCHAR(50) PRIMARY KEY
	);

En MariaDB:

![](Img/ejercicio1-10.PNG)

##### Resultado tablas #####
Tras acabar de crear las tablas, antes de ir a crear los `ALTER` comprobaremos que están todas creadas con el comando `SHOW TABLES`:

![](Img/ejercicio1-11.PNG)

#### ALTER TABLES <a name="AT"></a> ####
Despues de crear todas las tablas, usaremos ALTER para añadir los datos para relacionarlas entre ellas y para acabar los check.

##### Departamento #####
En departamento tendremos que añadir la columna de director y luego hacerla una FK de la tabla Profesor.

	ALTER TABLE Departamento ADD COLUMN Director CHAR(9)
	ALTER TABLE Departamento ADD FOREIGN KEY (Director) REFERENCES Profesor (DNI)
								 ON UPDATE CASCADE
								 ON DELETE SET NULL
En MariaDB:

![](Img/ejercicio1-12.PNG)

##### Grupo investigación #####
	
	ALTER TABLE Grupo_Investigacion ADD COLUMN Líder CHAR(9);
	ALTER TABLE Grupo_Investigacion ADD CONSTRAINT FK_Profesor_Grupo FOREIGN KEY (Líder) REFERENCES Profesor(DNI)
                                                   				     ON UPDATE CASCADE
                                                   					 ON DELETE SET NULL;
En MariaDB:
![](Img/ejercicio1-13.PNG)

##### Profesor #####

	ALTER TABLE Profesor ADD CONSTRAINT Experiencia CHECK (Experiencia BETWEEN 1 AND 50);
En MariaDB:

![](Img/ejercicio1-14.PNG)

##### Participación #####

	ALTER TABLE Participacion ADD FOREIGN KEY (Codigo_Proyecto) REFERENCES Proyecto(Codigo_Proyecto) 
                                  ON UPDATE CASCADE
                                  ON DELETE NO ACTION;
	ALTER TABLE Participacion ADD CONSTRAINT Comprobacion_Fechas CHECK (Fecha_Incorporacion < Fecha_Cese);
En MariaDB:

![](Img/ejercicio1-15.PNG)

##### Proyecto #####

	ALTER TABLE Proyecto ADD CONSTRAINT Comprobacion_Presupuesto CHECK (Presupuesto > 0);
	ALTER TABLE Proyecto ADD CONSTRAINT Comprobacion_Fechas CHECK (Fecha_Inicio < Fecha_Fin);
	ALTER TABLE Proyecto ADD CONSTRAINT Unicidad_Nombre UNIQUE(Nombre_Proyecto);
En MariaDB:

![](Img/ejercicio1-16.PNG)

##### Financiación #####

	ALTER TABLE Financiacion ADD FOREIGN KEY (Nombre_Programa) REFERENCES Programa(Nombre_Programa) 
                                 ON UPDATE CASCADE
                                 ON DELETE CASCADE;
	ALTER TABLE Financiacion ADD CONSTRAINT Unicidad_Asociado UNIQUE(Numero_Asociado);
En MariaDB:

![](Img/ejercicio1-17.PNG)

Hasta aquí estaría acabado el ejercicio 1.

### Ejercicio 2 <a name="ej2"></a> ###
En este ejercicio seguiremos los mismos criterios que en el anterior.

#### Creación base de datos  <a name="CBDD2"></a> ####
Como hicimos en el ejercicio anterior creamos la base de datos.

![](Img/ejercicio2-1.PNG)

![](Img/ejercicio2-2.PNG)

#### Creación tablas <a name="CT2"></a> ####
Empezamos a  crear las tablas.

##### Servicio #####
	
	CREATE TABLE Servicio (
		Clave_Servicio CHAR(5),
		Nombre_Servicio VARCHAR(40),
		PRIMARY KEY (Clave_Servicio, Nombre_Servicio)
	);
En MariaDB:

![](Img/ejercicio2-3.PNG)

##### Dependencia #####

	CREATE TABLE Dependencia (
		Codigo_Dependencia CHAR (5),
		Nombre_Dependencia VARCHAR (40),
		Funcion VARCHAR (20),
		Localizacion VARCHAR (20),
		Clave_Servicio CHAR (5) NOT NULL,
		Nombre_Servicio VARCHAR(40),
		PRIMARY KEY (Codigo_Dependencia),
		Unique (Nombre_Dependencia),
		FOREIGN KEY (Clave_Servicio, Nombre_Servicio) REFERENCES Servicio (Clave_Servicio, Nombre_Servicio)
		ON UPDATE CASCADE
		ON DELETE CASCADE
	);
En MariaDB:

![](Img/ejercicio2-4.PNG)

##### Cámara #####

	CREATE TABLE Camara (
		Codigo_Dependencia CHAR(5),
		Categoria VARCHAR(40) NOT NULL,
		Capacidad INTEGER NOT NULL,
		PRIMARY KEY (Codigo_Dependencia),
		FOREIGN KEY (Codigo_Dependencia) REFERENCES Dependencia (Codigo_Dependencia)
    	ON UPDATE Cascade
    	ON DELETE Cascade
	);
En MariaDB:

![](Img/ejercicio2-5.PNG)

##### Tripulacion #####

	CREATE TABLE Tripulacion (
		Codigo_Tripulacion CHAR(5) PRIMARY KEY,
		Nombre_Tripulacion VARCHAR(40),
		Categoria CHAR(20) NOT NULL,
		Antiguedad INTEGER DEFAULT 0,
		Procedencia CHAR(20),
		Adm CHAR(20) NOT NULL,
		Codigo_Dependencia CHAR(5) NOT NULL,
		Codigo_Camara CHAR(5) NOT NULL,
		FOREIGN KEY (Codigo_Camara) REFERENCES Camara (Codigo_Dependencia) 
		ON UPDATE CASCADE
    	ON DELETE CASCADE,
		FOREIGN KEY (Codigo_Dependencia) REFERENCES Dependencia (Codigo_Dependencia)
    	ON UPDATE CASCADE
    	ON DELETE CASCADE
	);
En MariaDB:

![](Img/ejercicio2-6.PNG)

##### Planeta #####

	CREATE TABLE Planeta (
	Codigo_Planeta CHAR(5) PRIMARY KEY,
	Nombre_Planeta VARCHAR(40) NOT NULL UNIQUE,
	Galaxia CHAR(15) NOT NULL,
	Coordenadas CHAR(15) NOT NULL,
	UNIQUE(Coordenadas)	
	);
En MariaDB:

![](Img/ejercicio2-7.PNG)

##### Visita #####

	CREATE TABLE Visita (
		Codigo_Tripulacion CHAR(5),
		Codigo_Planeta CHAR(5),
		Fecha_Visita DATE,
		Tiempo INTEGER NOT NULL,
		PRIMARY KEY (Codigo_Tripulacion, Codigo_Planeta, Fecha_Visita),
		FOREIGN KEY (Codigo_Tripulacion) REFERENCES Tripulacion (Codigo_Tripulacion)
		ON UPDATE CASCADE
		ON DELETE CASCADE,
		FOREIGN KEY (Codigo_Planeta) REFERENCES Planeta (Codigo_Planeta)
		ON UPDATE CASCADE
		ON DELETE CASCADE 
	);
En MariaDB:

![](Img/ejercicio2-8.PNG)

##### Habita #####

	CREATE TABLE Habita ( 
		Codigo_Planeta CHAR(5),
		Nombre_Raza VARCHAR(40),
		Poblacion_Parcial INTEGER NOT NULL,
		PRIMARY KEY (Codigo_Planeta, Nombre_Raza),
		FOREIGN KEY (Codigo_Planeta) REFERENCES Planeta (Codigo_Planeta)
		ON UPDATE Cascade
		ON DELETE Cascade
	);
En MariaDB:

![](Img/ejercicio2-9.PNG)

##### RAZA #####

	CREATE TABLE Raza (
		Nombre_Raza VARCHAR(40)  PRIMARY KEY,
		Altura INTEGER NOT NULL, -- cm
		Anchura INTEGER NOT NULL, -- cm
		Peso INTEGER NOT NULL, -- g
		Poblacion_Total INTEGER NOT NULL
	);
En MariaDB:

![](Img/ejercicio2-10.PNG)

#### ALTER TABLES <a name="AT2"></a> ####
Despues de crear todas las tablas, usaremos ALTER para añadir los datos para relacionarlas entre ellas y para acabar los check.

##### Habita #####
Tras crear la tabla raza falta por crear una FOREIGN KEY en la tabla de habita.

	ALTER TABLE Habita
		ADD CONSTRAINT FK_Raza
		FOREIGN KEY (Nombre_Raza) REFERENCES Raza (Nombre_Raza)
		ON UPDATE CASCADE
		ON DELETE CASCADE;
En MariaDB:

![](Img/ejercicio2-11.PNG)

	ALTER TABLE HABITA
		ADD CONSTRAINT poblacion_mayor_0
		CHECK (poblacion_Parcial > 0);
En MariaDB:

![](Img/ejercicio2-13.PNG)

##### Camara #####

	ALTER TABLE Camara
		ADD CONSTRAINT Capacidad_mayor_0
		CHECK (capacidad > 0);
En MariaDB:

![](Img/ejercicio2-12.PNG)

##### Raza #####

	ALTER TABLE RAZA
		ADD CONSTRAINT altura_mayor_0
		CHECK (altura > 0),
		ADD CONSTRAINT anchura_mayor_0
		CHECK (anchura > 0),
		ADD CONSTRAINT peso_mayor_0
		CHECK (peso > 0),
		ADD CONSTRAINT poblacion_mayor_0
		CHECK (poblacion_total > 0);
En MariaDB:

![](Img/ejercicio2-14.PNG)

Hasta aquí el ejercicio 2.