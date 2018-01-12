

PRACTICA 6.1


CREATE TABLE CENTROS(
  Numce NUMBER(4) NOT NULL,
  Nomce VARCHAR2(25) NOT NULL UNIQUE,
  Dirce VARCHAR2(25),
  CONSTRAINT PK_CENTROS PRIMARY KEY(numce)
);

CREATE TABLE DEPARTAMENTOS(
  numde NUMBER(3) NOT NULL,
  numce NUMBER(4),
  direc NUMBER(3),
  tidir CHAR(1),
  presu NUMBER(3,1),
  depde NUMBER(3),
  NOMDE VARCHAR2(20),
  CONSTRAINT PK_DEPARTAMENTOS PRIMARY KEY(numde),
  CONSTRAINT FK1_DEPARTAMENTOS FOREIGN KEY(numce)
    REFERENCES CENTROS(numce)
    ON DELETE CASCADE
);

-- NO PUEDO DEFINIR LA FORÁNEA REFLEXIVA PORQUE
-- NO PUEDO HACER REFERENCIA A UNA TABLA QUE
-- NO EXISTE. LA CREO AHORA CON UN ALTER TABLE


ALTER TABLE DEPARTAMENTOS
ADD CONSTRAINT FK2_DEPARTAMENTOS
FOREIGN KEY(depde)
REFERENCES DEPARTAMENTOS(numde);


CREATE TABLE EMPLEADOS(
  Numem NUMBER(3) NOT NULL,
  Extel NUMBER(3),
  Fecna DATE,
  Fecin DATE,
  Salar NUMBER(5),
  Comis NUMBER(3),
  Numhi NUMBER(1),
  NOMEM VARCHAR2(10),
  Numde NUMBER(3),
  CONSTRAINT PK_EMPLEADOS PRIMARY KEY(numem),
  CONSTRAINT FK1_EMPLEADOS FOREIGN KEY(numde)
    REFERENCES DEPARTAMENTOS(numde)
    ON DELETE CASCADE
);


3) Inserta los siguientes datos en la tabla DEPARTAMENTOS.

INSERT INTO DEPARTAMENTOS
VALUES(100, 10,260,'P',72,NULL, 'DIRECCIÓN GENERAL');
INSERT INTO DEPARTAMENTOS
VALUES(110, 20,180,'P',90,100, 'DIRECC.COMERCIAL');
INSERT INTO DEPARTAMENTOS
VALUES(111, 20,180,'F',66,110, 'SECTOR INDUSTRIAL');
INSERT INTO DEPARTAMENTOS
VALUES(112, 20,270,'P',54,110, 'SECTOR SERVICIOS');
INSERT INTO DEPARTAMENTOS
VALUES(120, 10,150,'F',18,100, 'ORGANIZACIÓN');
INSERT INTO DEPARTAMENTOS
VALUES(121, 10,150,'P',12,120, 'PERSONAL');
INSERT INTO DEPARTAMENTOS
VALUES(122, 10,350,'P',36,120, 'PROCESO DE DATOS');
INSERT INTO DEPARTAMENTOS
VALUES(130, 10,310,'P',12,100, 'FINANZAS');

4) ¿Qué ocurre al insertar el primer registro? ¿Por qué? Plantea la solución


5) Inserta los siguientes datos en la tabla CENTROS
INSERT INTO CENTROS VALUES(10,'SEDE CENTRAL','C/ ATOCHA, 820, MADRID');
INSERT INTO CENTROS VALUES(20,'RELACIÓN CON CLIENTES','C/ ATOCHA, 405, MADRID');


6) Descargate el script que te proporcionarán en clase para cargar los datos de la tabla EMPLEADOS.
















Consultas Sencillas 

1)Hallar, por orden alfabético, los nombres de los departamentos cuyo director lo es en funciones y no en propiedad.

SELECT nomde
FROM DEPARTAMENTOS
WHERE tidir = 'F'
ORDER BY 1;


2)Obtener un listín telefónico de los empleados del departamento 121 incluyendo nombre de empleado, número de empleado y extensión telefónica. Por orden alfabético.

SELECT Nomem , Numem , Extel
FROM EMPLEADOS
WHERE Numde = 121
ORDER BY 1;


3) Obtener por orden creciente una relación de todos los números de extensiones telefónicas de los empleados, junto con el nombre de estos, para aquellos que trabajen en el departamento 110. Mostrar la consulta tal y como aparece en la imagen.

SELECT nomem "Nombre", extel AS "Extensión Telefónica"
FROM EMPLEADOS
WHERE numde = 110
ORDER BY 1;

4) Hallar la comisión, nombre y salario de los empleados que tienen tres hijos, clasificados por comisión, y dentro de comisión por orden alfabético.

SELECT Comis,Nomem,Salar
FROM EMPLEADOS
WHERE Numhi = 3
ORDER BY 1, 2;

5) Hallar la comisión, nombre y salario de los empleados que tienen tres hijos, clasificados por comisión, y dentro de comisión por orden alfabético, para aquellos empleados que tienen comisión.

SELECT Comis, Nomem, Salar
FROM EMPLEADOS
WHERE Numhi = 3 AND comis IS NOT NULL
ORDER BY 1, 2;



6) Obtener salario y nombre de los empleados sin hijos y cuyo salario es mayor que 1200 y menor que 1500 €. Se obtendrán por orden decreciente de salario y por orden alfabético dentro de salario.

SELECT Salar,Nomem
FROM EMPLEADOS
WHERE Numhi = 0 AND salar > 1200 AND salar < 1500
ORDER BY 1 DESC, 2;


7) Obtener los números de los departamentos donde trabajan empleados cuyo salario sea inferior a 1500 €

SELECT DISTINCT Numde
FROM EMPLEADOS
WHERE Salar < 1500
ORDER BY 1;


8) Obtener las distintas comisiones que hay en el departamento 110.
SELECT DISTINCT Comis
FROM EMPLEADOS
WHERE Numde = 110;

Consultas con Predicados BETWEEN

1) Obtener por orden alfabético los nombres de los empleados cuyo salario está entre 1500€ y 1600 €

SELECT NONEM 
FROM EMPLEADOS
WHERE salar between 1500 and 1600
	ORDER BY 1;



2) Obtener por orden alfabético los nombres y salarios de los empleados con comisión,
	cuyo salario dividido por su número de hijos cumpla una, o ambas, de las dos 	condiciones
	siguientes:


SELECT nomem, salar
FROM EMPLEADOS
WHERE salar NOT BETWEEN 720*numhi AND 50*comis*numhi
AND comis IS NOT NULL
ORDER BY 1; 










Consultas con Predicados LIKE 


1) Obtener por orden alfa el nombre y el salario de aquellos empleados que comienzan por
	la letra 'A' y muestra la consulta como aparece en la captura.

	SELECT NOMEM
	FROM EMPLEADOS
	WHERE LIKE ‘A%’


2) Obtener por orden alfabético los nombres de los empleados que tengan 8 letras.

3) Obtener por orden alfabético los nombres y el presupuesto de los departamentos que incluyen la palabra “SECTOR”. La consulta la deberás mostrar como la imagen

Consultas con Predicados IN 

1) Obtener por orden alfabético los nombres de los empleados cuya extensión telefónica es
	250 o 750

	SELECT NONEM
	FROM EMPLEADOS
	WHERE
	WYHERE EXTEL IN (250,270)



2) Obtener por orden alfabético los nombres de los empleados que trabajan en el mismo
	departamento que PILAR o DOROTEA

	SELECT NONEM
	FROM EMPLEADOS
	WHERE NUMDE IN (
		SELECT NUMDE
		FROM EMPLEADOS
		WHERE NOMEM IN (`PILAR` , ‘DOROTEA’))
		ORDER BY 1;












3) Obtener por orden alfabético los nombres de los departamentos cuyo director es el
mismo que el del departamento: DIRECC.COMERCIAL o el del departamento:
PERSONAL Mostrar la consulta como la imagen.

SELECT NOMDE “Nombres Departamentos” direc “Identificador de su director”
FROM DEPARTAMENTOS
WHERE direc IN (
	SELECT direc 
	FROM DEPARTAMENTOS
	WHERE NOMDE IN (‘DIRECC.COMERCIAL’ , ‘PERSONAL’));

Consultas con Predicados EXISTS 
1) Obtener los nombres de los centros de trabajo si hay alguno que esté en la calle
ATOCHA
SELECT NOMEM, SALAR
FROM EMPLEADOS
WHERE NUMDE = 100 AND EXISTS (

	SELECT *
	FROM EMPLEADOS
	WHERE SALAR >1300 and numde = 100;
	)

2) Obtener los nombres y el salario de los empleados del departamento 100 si en él hay
alguno que gane más de 1300 €. 
