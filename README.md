-- CREACION TABLA EMPLEADO
CREATE TABLE empleado (
	id serial,
	nombre_empleado varchar(50) NOT NULL,
	apellido_empleado varchar(50) NOT NULL,
	cuil_empleado bigint NOT NULL,
	fecha_nacimiento_empleado date NOT NULL,
	direccion_empleado varchar(100) NOT NULL,
	email_empleado varchar(50) NOT NULL,
	contraseña_empleado varchar(20) NOT NULL,
	CONSTRAINT id_empleado PRIMARY KEY (id),
	fecha_registro timestamp
);

-- CREACION TABLA PUESTOS
CREATE TABLE puestos (
	id serial,
	nombre_puesto varchar(50) NOT NULL,
	sueldo_puesto double precision NOT NULL,
	CONSTRAINT id_puesto PRIMARY KEY (id)
);

-- CREACION TABLA RELACION EMPLEADO-PUESTO
CREATE TABLE empleado_puesto (
	id_empleado_r int NOT NULL,
	id_puesto_r int NOT NULL,
	CONSTRAINT empleado_puesto_pkey PRIMARY KEY (id_empleado_r,id_puesto_r),
	CONSTRAINT id_empleado_fkey FOREIGN KEY (id_empleado_r) REFERENCES empleado (id),
	CONSTRAINT id_puesto_fkey FOREIGN KEY (id_puesto_r) REFERENCES puestos (id)
);

-- CREACION TABLA LOGS DE PUESTOS

CREATE TABLE logs_puestos (
	id_empleado int NOT NULL,
	id_puesto int NOT NULL,
	fecha_inicio_puesto timestamp,
	fecha_fin_puesto timestamp,
	CONSTRAINT id_empleado_fkey FOREIGN KEY (id_empleado) REFERENCES empleado (id),
	CONSTRAINT id_puesto_fkey FOREIGN KEY (id_puesto) REFERENCES puestos (id)
);

-- CREACION TABLA TIPO DE CLIENTE
CREATE TABLE tipo_cliente (
	id serial,
	descripcion_tipo_cliente varchar(30),
	CONSTRAINT id_tipo_cliente PRIMARY KEY (id)
);

-- CREACION TABLA CLIENTE
CREATE TABLE cliente (
	id serial,
	nombre_cliente varchar(50) NOT NULL,
	apellido_cliente varchar(50) NOT NULL,
	cuil_cliente bigint NOT NULL,
	direccion_cliente varchar(100),
	telefono_cliente bigint,
	tipo_cliente int,
	CONSTRAINT id_cliente PRIMARY KEY (id),
	CONSTRAINT id_tipo_cliente_fkey FOREIGN KEY (tipo_cliente) REFERENCES tipo_cliente (id),
);

-- CREACION TABLA MARCA
CREATE TABLE marca (
	id serial,
	descripcion_marca varchar(30),
	CONSTRAINT id_marca PRIMARY KEY (id)
);

-- CREACION TABLA RUBRO
CREATE TABLE rubro (
	id serial,
	descripcion_rubro varchar(30),
	CONSTRAINT id_rubro PRIMARY KEY (id)
);

-- CREACION TABLA PRODUCTO
CREATE TABLE producto (
	id serial,
	nombre_producto varchar(50) NOT NULL,
	precio_compra double precision,
	precio_venta double precision NOT NULL,
	ean_producto int NOT NULL,
	marca_producto int NOT NULL,
	rubro_producto int NOT NULL,
	CONSTRAINT ean_producto PRIMARY KEY (ean_producto),
	CONSTRAINT id_marca_fkey FOREIGN KEY (marca_producto) REFERENCES marca (id),
	CONSTRAINT id_rubro_fkey FOREIGN KEY (rubro_producto) REFERENCES rubro (id)
);

-- CREACION TABLA EMPRESA PROVEEDORA
CREATE TABLE empresa_proveedora (
	id serial,
	nombre_empresa varchar(50) NOT NULL,
	cuit_empresa bigint NOT NULL,
	direccion_empresa varchar(100) NOT NULL,
	telefono_empresa bigint NOT NULL,
	email_empresa varchar(50) NOT NULL,
	CONSTRAINT id_empresa PRIMARY KEY (id)
);

-- CREACION TABLA TIPO DE PAGO
CREATE TABLE tipo_pago (
	id serial,
	descripcion_pago varchar(40),
	CONSTRAINT id_pago PRIMARY KEY (id)
);
-- CREACION TABLA DETALLE COMPRA TEMPORAL
CREATE TABLE detalle_compra_temporal(
	numero_orden bigint DEFAULT 1,
	ean_producto_compra bigint,
	cantidad_producto int,
	precio_unitario double precision,
	total double precision,
	CONSTRAINT ean_producto_compra_fkey FOREIGN KEY (ean_producto_compra) REFERENCES producto (ean_producto)
);

-- CREACION TABLA DETALLE COMPRA
CREATE TABLE detalle_compra(
	id serial,
	ean_producto_compra bigint,
	cantidad_producto int,
	precio_unitario double precision,
	total_fila double precision,
	numero_orden bigint
);

-- CREACION TABLA COMPRA DE MERCADERIA
CREATE TABLE compra_mercaderia (
	id serial,
	numero_factura varchar (50) NOT NULL,
	id_empresa int NOT NULL,
	detalle_compra serial,
	monto_compra double precision NOT NULL,
	tipo_pago_compra_mercaderia int NOT NULL,
	id_empleado_compra int NOT NULL,
	CONSTRAINT id_compra_mercaderia PRIMARY KEY (id),
	CONSTRAINT id_empresa_fkey FOREIGN KEY (id_empresa) REFERENCES empresa_proveedora (id),
	CONSTRAINT id_empleado_compra_fkey FOREIGN KEY (id_empleado_compra) REFERENCES empleado (id),
	CONSTRAINT id_tipo_pago_compra_mercaderia_fkey FOREIGN KEY (tipo_pago_compra_mercaderia) REFERENCES tipo_pago (id),
	fecha timestamp
);

-- CREACION TABLA DETALLE VENTA TEMPORAL
CREATE TABLE detalle_venta_temporal (
	numero_orden bigint DEFAULT 1,
	ean_producto_venta bigint,
	cantidad_producto int,
	precio_unitario_venta double precision,
	subtotal double precision,
	CONSTRAINT ean_producto_venta_fkey FOREIGN KEY (ean_producto_venta) REFERENCES producto (ean_producto)
);

-- CREACION TABLA DETALLE VENTA
CREATE TABLE detalle_venta(
	id serial,
	numero_orden bigint,
	ean_producto_venta bigint,
	cantidad_producto int,
	precio_unitario_venta double precision,
	total_fila double precision,
	total_factura double precision
);

-- CREACION TABLA VENTA DE MERCADERIA
CREATE TABLE venta_mercaderia (
	numero_factura_venta serial,
	monto_venta double precision NOT NULL,
	id_cliente_venta int NOT NULL,
	id_empleado_venta int NOT NULL,
	tipo_pago_venta_mercaderia int NOT NULL,
	detalle_venta serial,
	CONSTRAINT id_venta_mercaderia PRIMARY KEY (numero_factura_venta),
	CONSTRAINT id_cliente_venta_fkey FOREIGN KEY (id_cliente_venta) REFERENCES cliente (id),
	CONSTRAINT id_empleado_venta_fkey FOREIGN KEY (id_empleado_venta) REFERENCES empleado (id),
	CONSTRAINT id_tipo_pago_venta_mercaderia_fkey FOREIGN KEY (tipo_pago_venta_mercaderia) REFERENCES tipo_pago (id),
	fecha_venta timestamp
);

-- CREACION TABLA LOGS DE COMPRAS
CREATE TABLE logs_compras (
	id int,
	monto_compra int,
	fecha_compra timestamp
);

-- CREACION TABLA LOGS DE VENTAS
CREATE TABLE logs_ventas (
	id int,
	monto_venta int,
	tipo_pago_venta_mercaderia varchar(20),
	fecha_venta timestamp
);

-- CREACION TABLA LOGS DE EMPLEADOS
CREATE TABLE logs_empleado (
	nombre_empleado_l varchar (50),
	apellido_empleado_l varchar (50),
	cuil_empleado_l bigint,
	fecha_nacimiento_empleado_l date,
	direccion_empleado_l varchar (100),
	email_empleado_l varchar (50),
	contraseña_empleado_l varchar(20),
	fecha_modificacion_l timestamp

);

-- CREACION TABLA LOGS DE PRODUCTO
CREATE TABLE logs_producto (
	ean_producto_l bigint,
	nombre_producto varchar (50),
	precio_compra_l double precision,
	precio_venta_l double precision,
	marca_producto_l int,
	rubro_producto_l int,
	estado_l varchar (30),
	usuario_l name
);



/***************************************************************************************************************
****************************************************************************************************************
****************************************************************************************************************
*/


-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR DETALLE COMPRA TEMPORAL
CREATE OR REPLACE FUNCTION cargar_detalle_compra_temporal (ean_producto_compra bigint,
														   cantidad_producto int)
RETURNS void AS
$$
DECLARE
	numero_detalle bigint;
	preciodecompra double precision;
BEGIN	
	numero_detalle := (SELECT MAX (detalle_compra) FROM compra_mercaderia)+1;
	if numero_detalle = NULL then 
	numero_detalle = 1; end if;
	preciodecompra := (SELECT (precio_compra) FROM producto WHERE ean_producto = ean_producto_compra);
INSERT INTO detalle_compra_temporal(ean_producto_compra, cantidad_producto, precio_unitario, numero_orden) 
VALUES (ean_producto_compra, cantidad_producto, preciodecompra, numero_detalle);
END;
$$
LANGUAGE 'plpgsql';
-- CREACION FUNCION PARA CARGAR LA COLUMNA TOTAL
CREATE OR REPLACE FUNCTION sumar_total_fila()
RETURNS TRIGGER AS
$$
BEGIN
UPDATE detalle_compra_temporal SET total = cantidad_producto*precio_unitario;
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE TRIGGER sumar_total_fila
AFTER INSERT
ON detalle_compra_temporal
FOR EACH ROW
EXECUTE PROCEDURE sumar_total_fila();

SELECT cargar_detalle_compra_temporal (7790070562258, 100);

-- CREACION PROCEDIMIENTO ALMACENADO PARA ACTUALIZAR DETALLE COMPRA TEMPORAL
CREATE OR REPLACE FUNCTION actualizar_detalle_compra_temporal (ean_producto_compra_a bigint,
															   cantidad_producto_a int,
															   id_ubicacion int)
RETURNS void AS $$
DECLARE
	ean_buscar_cmp_temp bigint;
BEGIN
	ean_buscar_cmp_temp := (SELECT (ean_producto) FROM producto where ean_producto=ean_producto_compra_a);
	if (ean_buscar_cmp_temp = ean_producto_compra_a ) then 
	UPDATE detalle_compra_temporal SET cantidad_producto = cantidad_producto_a where id=id_ubicacion;
 	end if;
END;
$$
LANGUAGE 'plpgsql';

SELECT actualizar_detalle_compra_temporal(7790742357205, 20, 1);

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN DETALLE DE COMPRA TEMPORAL
CREATE OR REPLACE FUNCTION eliminar_detalle_compra_temporal_1 (ean_producto_compra_e bigint,
															   id_ubicacion int)
RETURNS void AS $$
DECLARE
	ean_buscar_cmp_temp bigint;
BEGIN
	ean_buscar_cmp_temp := (SELECT (ean_producto) FROM producto where ean_producto=ean_producto_compra_e);
	if (ean_buscar_cmp_temp = ean_producto_compra_e ) then 
	DELETE FROM detalle_compra_temporal WHERE  ean_producto_compra = ean_producto_compra_e and id=id_ubicacion;
 	end if;
END;
$$
LANGUAGE 'plpgsql';
SELECT eliminar_detalle_compra_temporal_1 (7790742357205, 2);


-- CREACION FUNCION PARA CARGAR LA TABLA DETALLE DE COMPRA  Y ACTUALIZACION DE LA TABLA
-- DETALLE DE COMPRA TEMPORAL
CREATE OR REPLACE FUNCTION cargar_compra_mercaderia ()
RETURNS TRIGGER AS
$$
BEGIN
INSERT INTO detalle_compra (ean_producto_compra, cantidad_producto, precio_unitario, total_fila, numero_orden) 
SELECT ean_producto_compra, cantidad_producto, precio_unitario, total, numero_orden FROM detalle_compra_temporal;
DELETE FROM detalle_compra_temporal;
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE TRIGGER cargar_compra_mercaderia
AFTER INSERT
ON compra_mercaderia
FOR EACH ROW
EXECUTE PROCEDURE cargar_compra_mercaderia();


-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR LA TABLA DETALLE VENTA TEMPORAL
CREATE OR REPLACE FUNCTION cargar_detalle_venta_temporal( ean_producto_venta bigint,
														  cantidad_producto int)
RETURNS void AS
$$
DECLARE
	numero_detalle_venta bigint;
	preciouvta double precision;
BEGIN	
	numero_detalle_venta := (SELECT MAX (detalle_venta) FROM venta_mercaderia)+1;
	if numero_detalle_venta = NULL then 
	numero_detalle_venta = 1; end if;
	preciouvta := (SELECT (precio_venta) FROM producto WHERE ean_producto= ean_producto_venta);
INSERT INTO detalle_venta_temporal(numero_orden, ean_producto_venta, cantidad_producto, precio_unitario_venta) 
VALUES (numero_detalle_venta, ean_producto_venta, cantidad_producto, preciouvta);
END;
$$
LANGUAGE 'plpgsql';
SELECT cargar_detalle_venta_temporal(7790742357205, 25);

-- CREACION FUNCION PARA CARGAR LA COLUMNA SUBTOTAL
CREATE OR REPLACE FUNCTION sumar_total_fila_venta()
RETURNS TRIGGER AS
$$
BEGIN
UPDATE detalle_venta_temporal SET subtotal = cantidad_producto*precio_unitario_venta;
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER sumar_total_fila_venta
AFTER INSERT
ON detalle_venta_temporal
FOR EACH ROW
EXECUTE PROCEDURE sumar_total_fila_venta();


-- CREACION FUNCION PARA CARGAR LA TABLA DETALLE DE VENTA  Y ACTUALIZACION DE LA TABLA
-- DETALLE DE VENTA TEMPORAL
CREATE OR REPLACE FUNCTION cargar_venta_mercaderia ()
RETURNS TRIGGER AS
$$
BEGIN
INSERT INTO detalle_venta (numero_orden, ean_producto_venta, cantidad_producto, precio_unitario_venta, total_fila, total_factura) 
SELECT numero_orden, ean_producto_venta, cantidad_producto, precio_unitario_venta,subtotal, (SELECT SUM (subtotal) FROM detalle_venta_temporal) 
FROM detalle_venta_temporal;
DELETE FROM detalle_venta_temporal;
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER cargar_venta_mercaderia
AFTER INSERT
ON venta_mercaderia
FOR EACH ROW
EXECUTE PROCEDURE cargar_venta_mercaderia();


-- CREACION FUNCION PARA CARGAR TABLA LOGS_COMPRAS
CREATE OR REPLACE FUNCTION logs_compras_trigger_fnc()
RETURNS trigger AS
$$
BEGIN
INSERT INTO logs_compras (id, monto_compra, fecha_compra)
VALUES
(NEW.id, NEW.monto_compra, NOW());
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE TRIGGER logs_compras_trigger_fnc
AFTER INSERT
ON compra_mercaderia
FOR EACH ROW
EXECUTE PROCEDURE logs_compras_trigger_fnc();


-- CREACION FUNCION PARA CARGAR TABLA LOGS_VENTAS
CREATE OR REPLACE FUNCTION logs_ventas_trigger_fnc()
RETURNS trigger AS
$$
BEGIN
INSERT INTO logs_ventas (id, monto_venta, tipo_pago_venta_mercaderia, fecha_venta)
VALUES
(NEW.numero_factura_venta, NEW.monto_venta, NEW.tipo_pago_venta_mercaderia, NOW());
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE TRIGGER logs_ventas_trigger_fnc
AFTER INSERT
ON venta_mercaderia
FOR EACH ROW
EXECUTE PROCEDURE logs_ventas_trigger_fnc();

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA EMPLEADO
CREATE OR REPLACE FUNCTION cargar_empleado(nombre_empleado_1 varchar(50),
										   apellido_empleado_1 varchar(50),
										   cuil_empleado_1 bigint,
										   fecha_nacimiento_empleado_1 date,
										   direccion_empleado_1 varchar(100),
										   email_empleado_1 varchar(50),
										   contraseña_empleado_1 varchar(20))
RETURNS void AS $$
declare
	fecha_registro_empleado timestamp;
BEGIN
	fecha_registro_empleado := (SELECT NOW());
	if (cuil_empleado_1 NOT IN (SELECT cuil_empleado FROM empleado)) then
	INSERT INTO empleado (nombre_empleado, apellido_empleado, cuil_empleado, fecha_nacimiento_empleado,
	direccion_empleado, email_empleado, contraseña_empleado, fecha_registro)
	VALUES (nombre_empleado_1, apellido_empleado_1, cuil_empleado_1, fecha_nacimiento_empleado_1,
	direccion_empleado_1, email_empleado_1, contraseña_empleado_1,fecha_registro_empleado);
	else UPDATE empleado SET nombre_empleado=nombre_empleado_1, apellido_empleado=apellido_empleado_1,
	cuil_empleado=cuil_empleado_1, fecha_nacimiento_empleado=fecha_nacimiento_empleado_1,direccion_empleado=direccion_empleado_1, 
	email_empleado=email_empleado_1, contraseña_empleado=contraseña_empleado_1 
	WHERE cuil_empleado=cuil_empleado_1; end if;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_empleado ('Ernesto', 'Gonzalez', 20251234566, '1992/08/25', 'Belgrano 235',
						'ernestogonzalez@gmail.com', '455');

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA PUESTO
CREATE OR REPLACE FUNCTION cargar_puesto (nombre_puesto_1 varchar(50), sueldo_puesto_1 double precision)
RETURNS void AS $$
BEGIN
	if ( lower (nombre_puesto_1) not in (SELECT nombre_puesto from puestos WHERE lower (nombre_puesto)= lower (nombre_puesto_1))) then
	INSERT INTO puestos (nombre_puesto, sueldo_puesto) VALUES (nombre_puesto_1, sueldo_puesto_1);
	else UPDATE puestos SET nombre_puesto=nombre_puesto_1, sueldo_puesto=sueldo_puesto_1 WHERE lower (nombre_puesto)= lower (nombre_puesto_1);
	end if;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_puesto ('hOla', 490000);

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA PUESTOS
CREATE OR REPLACE FUNCTION eliminar_puesto (nombre_puesto_e varchar(50), sueldo_puesto_e double precision)
RETURNS void AS $$
BEGIN
	DELETE FROM puestos WHERE lower (nombre_puesto)= lower (nombre_puesto_e) AND sueldo_puesto=sueldo_puesto_e;
END
$$ LANGUAGE 'plpgsql';

SELECT sueldo_puesto_e('HOLA', 420000);

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR TABLA EMPLEADO_PUESTO
CREATE OR REPLACE FUNCTION cargar_empleado_puesto(id_empleado_c int, id_puesto_c int)
RETURNS void AS $$
BEGIN
INSERT INTO empleado_puesto (id_empleado_r, id_puesto_r) VALUES (id_empleado_c, id_puesto_c);
END;
$$
LANGUAGE 'plpgsql';
SELECT cargar_empleado_puesto(7,4);

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA EMPLEADO_PUESTO
CREATE OR REPLACE FUNCTION eliminar_empleado_puesto( numero_relacion_e int)
RETURNS void AS $$
BEGIN
DELETE FROM empleado_puesto WHERE (numero_relacion_r = numero_relacion_e);
END
$$
LANGUAGE 'plpgsql';
SELECT eliminar_empleado_puesto(5);


-- CREACION TABLA TEMPORAL PARA CARGAR LOGS DE PUESTOS ANTE UNA ELIMINACION
CREATE TABLE puesto_liberado (
	numero_relacion_eliminada int
);
INSERT INTO puesto_liberado (numero_relacion_eliminada) VALUES (1);
-- CREACION FUNCION PARA CARGAR TABLA PUESTO_LIBERADO
CREATE OR REPLACE FUNCTION puesto_liberado_trigger_fnc()
RETURNS trigger AS
$$
BEGIN
UPDATE puesto_liberado SET numero_relacion_eliminada=OLD.numero_relacion_r;
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER puesto_liberado_trigger_fnc
BEFORE DELETE
ON empleado_puesto
FOR EACH ROW
EXECUTE PROCEDURE puesto_liberado_trigger_fnc();



-- CREACION FUNCION PARA CARGAR TABLA LOGS DE PUESTOS
CREATE OR REPLACE FUNCTION logs_puestos_trigger_fnc()
RETURNS trigger AS
$$
BEGIN
INSERT INTO logs_puestos (id_empleado, id_puesto, numero_relacion_logs, fecha_inicio_puesto)
VALUES
(NEW.id_empleado_r, NEW.id_puesto_r, NEW.numero_relacion_r, NOW());
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER logs_puestos_trigger_fnc
AFTER INSERT
ON empleado_puesto
FOR EACH ROW
EXECUTE PROCEDURE logs_puestos_trigger_fnc();

-- CREACION FUNCION PARA ACTUALIZAR TABLA LOGS DE PUESTOS AL FINZALIZAR UNA RELACION EMPLEADO-PUESTO
CREATE OR REPLACE FUNCTION logs_puestos_act_trigger_fnc()
RETURNS trigger AS
$$
DECLARE
	fecha_baja timestamp;
	numero_relacion_baja int;
BEGIN
	fecha_baja := (SELECT NOW());
	numero_relacion_baja := (SELECT (numero_relacion_eliminada) FROM puesto_liberado);
UPDATE logs_puestos SET  fecha_fin_puesto=fecha_baja WHERE numero_relacion_logs=numero_relacion_baja;
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER logs_puestos_act_trigger_fnc
AFTER UPDATE 
ON puesto_liberado
FOR EACH ROW
EXECUTE PROCEDURE logs_puestos_act_trigger_fnc();



-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA TIPO DE CLIENTE
CREATE OR REPLACE FUNCTION cargar_tipo_cliente(descripcion_tipo_cliente_1 varchar(30))
RETURNS void AS $$
BEGIN
	IF (lower (descripcion_tipo_cliente_1) NOT IN (SELECT lower (descripcion_tipo_cliente) FROM tipo_cliente )) THEN
INSERT INTO tipo_cliente (descripcion_tipo_cliente) VALUES (descripcion_tipo_cliente_1);
	ELSE UPDATE tipo_cliente SET descripcion_tipo_cliente=descripcion_tipo_cliente_1
	WHERE lower(descripcion_tipo_cliente)= lower (descripcion_tipo_cliente_1); END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_tipo_cliente ('Pepito');

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO TABLA TIPO DE CLIENTE
CREATE OR REPLACE FUNCTION eliminar_tipo_cliente(descripcion_tipo_cliente_e varchar(30))
RETURNS void AS $$
BEGIN
	IF (lower (descripcion_tipo_cliente_e) = (SELECT lower (descripcion_tipo_cliente) FROM tipo_cliente WHERE lower (descripcion_tipo_cliente)= lower (descripcion_tipo_cliente_e))) THEN
DELETE FROM tipo_cliente WHERE lower(descripcion_tipo_cliente)= lower (descripcion_tipo_cliente_e); END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT eliminar_tipo_cliente ('pepito');


-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA CLIENTE
CREATE OR REPLACE FUNCTION cargar_cliente(nombre_cliente_c varchar(50),
										  apellido_cliente_c varchar(50),
										  cuil_cliente_c bigint,
										  direccion_cliente_c varchar(100),
										  telefono_cliente_c bigint,
									   	  tipo_cliente_c int)
RETURNS void AS $$
BEGIN
	if (cuil_cliente_c) NOT IN (SELECT (cuil_cliente) FROM cliente) THEN
	INSERT INTO cliente (nombre_cliente, apellido_cliente, cuil_cliente, direccion_cliente,
	telefono_cliente, tipo_cliente)
	VALUES (nombre_cliente_c, apellido_cliente_c, cuil_cliente_c,
	direccion_cliente_c, telefono_cliente_c, tipo_cliente_c);
	ELSE UPDATE cliente SET nombre_cliente=nombre_cliente_c, apellido_cliente=apellido_cliente_c,
	cuil_cliente=cuil_cliente_c, direccion_cliente=direccion_cliente_c,
	telefono_cliente=telefono_cliente_c, tipo_cliente=tipo_cliente_c WHERE cuil_cliente=cuil_cliente_c;
	END IF;
END;
$$
LANGUAGE 'plpgsql';
SELECT cargar_cliente ('Banco Formosa ',' ', 20401234566,'25 de Mayo 920', 1704005285, 3);

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA CLIENTE
CREATE OR REPLACE FUNCTION eliminar_cliente(cuil_cliente_e bigint)
RETURNS void AS $$
BEGIN
	IF ( cuil_cliente_e = (SELECT (cuil_cliente) FROM cliente WHERE cuil_cliente= cuil_cliente_e)) THEN
DELETE FROM cliente WHERE cuil_cliente= cuil_cliente_e; END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT eliminar_cliente ('20401234566');

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA MARCA
CREATE OR REPLACE FUNCTION cargar_tipo_marca(descripcion_marca_c varchar(30))
RETURNS void AS $$
BEGIN
	IF lower(descripcion_marca_c) NOT IN (SELECT lower(descripcion_marca) FROM marca) THEN 
INSERT INTO marca (descripcion_marca) VALUES (descripcion_marca_c);
	ELSE UPDATE marca SET descripcion_marca=descripcion_marca_c WHERE lower (descripcion_marca)= lower (descripcion_marca_c);
END IF; 
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_tipo_marca ('Pepito');

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA MARCA
CREATE OR REPLACE FUNCTION eliminar_marca (descripcion_marca_e varchar(30))
RETURNS void AS $$
BEGIN
	IF (lower (descripcion_marca_e) = (SELECT lower (descripcion_marca) FROM marca WHERE lower (descripcion_marca)= lower (descripcion_marca_e))) THEN
DELETE FROM marca WHERE lower(descripcion_marca)= lower (descripcion_marca_e); END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT eliminar_marca ('pepito');


-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA RUBRO
CREATE OR REPLACE FUNCTION cargar_tipo_rubro (descripcion_rubro_c varchar(30))
RETURNS void AS $$
BEGIN
	IF lower(descripcion_rubro_c) NOT IN (SELECT lower(descripcion_rubro) FROM rubro) THEN
INSERT INTO rubro (descripcion_rubro) VALUES (descripcion_rubro_c);
	ELSE UPDATE rubro SET descripcion_rubro=descripcion_rubro_c WHERE lower (descripcion_rubro)= lower (descripcion_rubro_c);
END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_tipo_rubro ('Pepito');

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA RUBRO
CREATE OR REPLACE FUNCTION eliminar_rubro (descripcion_rubro_e varchar(30))
RETURNS void AS $$
BEGIN
	IF (lower (descripcion_rubro_e) = (SELECT lower (descripcion_rubro) FROM rubro WHERE lower (descripcion_rubro)= lower (descripcion_rubro_e))) THEN
DELETE FROM rubro WHERE lower(descripcion_rubro)= lower (descripcion_rubro_e); END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT eliminar_rubro ('pepito');

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA PRODUCTO
CREATE OR REPLACE FUNCTION cargar_producto (nombre_producto_c varchar(50),
										   precio_compra_c double precision,
										   precio_venta_c double precision,
										   ean_producto_c bigint,
										   marca_producto_c int,
										   rubro_producto_c int,
										   estado_producto_c varchar(30))
RETURNS void AS $$
BEGIN
	IF (ean_producto_c) NOT IN (SELECT (ean_producto) FROM producto) THEN
INSERT INTO producto (nombre_producto, precio_compra, precio_venta, ean_producto, marca_producto, rubro_producto, estado)
VALUES (nombre_producto_c, precio_compra_c, precio_venta_c, ean_producto_c, marca_producto_c, rubro_producto_c,estado_producto_c);
	ELSE UPDATE producto SET nombre_producto=nombre_producto_c, precio_compra=precio_compra_c, precio_venta=precio_venta_c,
	ean_producto=ean_producto_c, marca_producto=marca_producto_c, rubro_producto=rubro_producto_c, estado=estado_producto_c
	WHERE ean_producto=ean_producto_c;
END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_producto('LS LECHE ENTERA TETRA x1lt', 790.55, 995, 7790742357205, 1, 1, 'ACTIVO');

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA EMPRESA PROVEEDORA
CREATE OR REPLACE FUNCTION cargar_empresa_proveedora (nombre_empresa_c varchar(50),
													 cuit_empresa_c bigint,
													 direccion_empresa_c varchar(100),
													 telefono_empresa_c bigint,
													 email_empresa_c varchar(50))
RETURNS void AS $$
BEGIN
	IF (cuit_empresa_c) NOT IN (SELECT(cuit_empresa) FROM empresa_proveedora) THEN
INSERT INTO empresa_proveedora (nombre_empresa, cuit_empresa, direccion_empresa, telefono_empresa, email_empresa)
VALUES (nombre_empresa_c, cuit_empresa_c, direccion_empresa_c, telefono_empresa_c, email_empresa_c);
	ELSE UPDATE empresa_proveedora SET nombre_empresa=nombre_empresa_c, cuit_empresa=cuit_empresa_c, direccion_empresa=direccion_empresa_c,
	telefono_empresa=telefono_empresa_c, email_empresa=email_empresa_c WHERE cuit_empresa=cuit_empresa_c;
	END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_empresa_proveedora('Pepito S.A.', 30702223331,'Av. Italia 2500. Formosa Capital', 370442285, 'pepito@gmail.com');

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA EMPRESA PROVEEDORA
CREATE OR REPLACE FUNCTION eliminar_empresa_proveedora (cuit_empresa_e bigint)
RETURNS void AS $$
BEGIN
	IF ( cuit_empresa_e = (SELECT (cuit_empresa) FROM empresa_proveedora WHERE cuit_empresa= cuit_empresa_e)) THEN
DELETE FROM empresa_proveedora WHERE cuit_empresa= cuit_empresa_e; END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT eliminar_empresa_proveedora ('30702223331');


-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR O ACTUALIZAR TABLA TIPO DE PAGO
CREATE OR REPLACE FUNCTION cargar_tipo_pago (descripcion_pago_c varchar(40))
RETURNS void AS $$
BEGIN
	IF lower(descripcion_pago_c) NOT IN (SELECT lower(descripcion_pago) FROM tipo_pago) THEN
INSERT INTO tipo_pago (descripcion_pago) VALUES (descripcion_pago_c);
	ELSE UPDATE tipo_pago SET descripcion_pago=descripcion_pago_c WHERE lower (descripcion_pago)= lower(descripcion_pago_c);
	END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT cargar_tipo_pago('PEPITO');

-- CREACION PROCEDIMIENTO ALMACENADO PARA ELIMINAR UN REGISTRO EN TABLA TIPO DE PAGO
CREATE OR REPLACE FUNCTION eliminar_tipo_pago (descripcion_pago_e varchar(30))
RETURNS void AS $$
BEGIN
	IF (lower (descripcion_pago_e) = (SELECT lower (descripcion_pago) FROM tipo_pago WHERE lower (descripcion_pago)= lower (descripcion_pago_e))) THEN
DELETE FROM tipo_pago WHERE lower(descripcion_pago)= lower (descripcion_pago_e); END IF;
END;
$$
LANGUAGE 'plpgsql';

SELECT eliminar_tipo_pago ('pepito');


-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR TABLA COMPRA DE MERCADERIA
CREATE OR REPLACE FUNCTION cargar_compra_mercaderia(id_empresa int,
													tipo_pago_compra_mercaderia int,
												    id_empleado_compra int,
												    numero_factura varchar(50))
RETURNS void AS $$
DECLARE
	total_compra double precision;
	fecha_registro timestamp;
BEGIN
	total_compra := (SELECT SUM(total) FROM detalle_compra_temporal);
	fecha_registro := (SELECT NOW());
INSERT INTO compra_mercaderia (id_empresa, tipo_pago_compra_mercaderia, id_empleado_compra, monto_compra, fecha, numero_factura)
VALUES (id_empresa, tipo_pago_compra_mercaderia, id_empleado_compra, total_compra, fecha_registro, numero_factura);
END;
$$
LANGUAGE 'plpgsql';
SELECT cargar_compra_mercaderia (1, 1, 5, '0011-00002285');

-- CREACION PROCEDIMIENTO ALMACENADO PARA CARGAR TABLA VENTA DE MERCADERIA
CREATE OR REPLACE FUNCTION cargar_venta_mercaderia (id_cliente_venta int,
													tipo_pago_venta_mercaderia int,
												    id_empleado_venta int)
RETURNS void AS $$
DECLARE
	total_venta double precision;
	fecha_factura timestamp;
BEGIN
	total_venta := (SELECT SUM(subtotal) FROM detalle_venta_temporal);
	fecha_factura := (SELECT NOW());
INSERT INTO venta_mercaderia (monto_venta, id_cliente_venta, id_empleado_venta, tipo_pago_venta_mercaderia, fecha_venta)
VALUES (total_venta, id_cliente_venta, id_empleado_venta, tipo_pago_venta_mercaderia, fecha_factura);
END;
$$
LANGUAGE 'plpgsql';


SELECT cargar_venta_mercaderia(1,1,5);


-- CREACION TRIGGER PARA CARGAR LOGS DE EMPLEADO
CREATE OR REPLACE FUNCTION logs_empleado_trg ()
RETURNS TRIGGER 
AS
$$
DECLARE fecha_modificacion_empleado timestamp;
BEGIN
	fecha_modificacion_empleado := (SELECT NOW());
INSERT INTO logs_empleado (nombre_empleado_l, apellido_empleado_l, cuil_empleado_l,fecha_nacimiento_empleado_l,
						  direccion_empleado_l, email_empleado_l, contraseña_empleado_l, fecha_modificacion_l)
						  VALUES (NEW.nombre_empleado, NEW.apellido_empleado, NEW.cuil_empleado, NEW.fecha_nacimiento_empleado,
								 NEW.direccion_empleado, NEW.email_empleado,NEW.contraseña_empleado, fecha_modificacion_empleado);
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER logs_empleado_trg
AFTER INSERT OR UPDATE 
ON empleado
FOR EACH ROW
EXECUTE PROCEDURE logs_empleado_trg();



-- CREACION TRIGGER PARA CARGAR LOGS DE EMPLEADO
CREATE OR REPLACE FUNCTION logs_producto_trg ()
RETURNS TRIGGER 
AS
$$
DECLARE fecha_modificacion_producto timestamp;
BEGIN
	fecha_modificacion_producto := (SELECT NOW());
INSERT INTO logs_producto (ean_producto_l, nombre_producto_l, precio_compra_l, precio_venta_l,
						  marca_producto_l, rubro_producto_l, estado_l, usuario_l, fecha_modificacion_l)
						  VALUES (NEW.ean_producto, NEW.nombre_producto, NEW.precio_compra, NEW.precio_venta,
								 NEW.marca_producto, NEW.rubro_producto, NEW.estado, current_user, fecha_modificacion_producto);
RETURN NEW;
END;
$$
LANGUAGE 'plpgsql';
CREATE OR REPLACE TRIGGER logs_producto_trg
AFTER INSERT OR UPDATE 
ON producto
FOR EACH ROW
EXECUTE PROCEDURE logs_producto_trg();




/***************************************************************************************************
****************************************************************************************************
****************************************************************************************************
*/

--CREACION VISTA PARA SABER TOTAL DE GASTOS
CREATE OR REPLACE VIEW total_gastos
AS 
SELECT empresa_proveedora.nombre_empresa, tipo_pago.descripcion_pago, compra_mercaderia.fecha,
compra_mercaderia.numero_factura,compra_mercaderia.monto_compra, 
CASE WHEN ROW_NUMBER() OVER (ORDER BY compra_mercaderia.fecha ) =1 THEN SUM (monto_compra) OVER()
 ELSE NULL END as Total FROM compra_mercaderia
INNER JOIN empresa_proveedora ON empresa_proveedora.id = compra_mercaderia.id_empresa
INNER JOIN tipo_pago ON tipo_pago.id = compra_mercaderia.tipo_pago_compra_mercaderia

SELECT *FROM total_gastos;

--CREACION VISTA PARA SABER STOCK

CREATE OR REPLACE VIEW stock_mercaderia
AS
SELECT DISTINCT producto.ean_producto, producto.nombre_producto, SUM(detalle_compra.cantidad_producto) AS cantidad_compra,
SUM(detalle_venta.cantidad_producto) AS cantidad_venta, SUM(detalle_compra.cantidad_producto) - SUM(detalle_venta.cantidad_producto) AS STOCK FROM producto
INNER JOIN detalle_compra  ON detalle_compra.ean_producto_compra = producto.ean_producto
INNER JOIN detalle_venta ON detalle_venta.ean_producto_venta= producto.ean_producto
GROUP BY ean_producto

SELECT * FROM stock_mercaderia



/**************************************************************************************************
***************************************************************************************************
*/

CREATE ROLE readonly;

GRANT CONNECT ON DATABASE supermercadojj TO readonly;

GRANT USAGE ON SCHEMA public TO readonly;

GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;

ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO readonly;


CREATE ROLE readwrite;

GRANT CONNECT ON DATABASE supermercadojj TO readwrite;


GRANT USAGE ON SCHEMA public TO readwrite;


GRANT USAGE, CREATE ON SCHEMA public TO readwrite;

GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO readwrite;

ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO readwrite;

GRANT USAGE ON ALL SEQUENCES IN SCHEMA public TO readwrite;

ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT USAGE ON SEQUENCES TO readwrite;

CREATE USER usuario1 WITH PASSWORD '123456';
GRANT readonly TO usuario1;

CREATE USER usuario2 WITH PASSWORD '55555';
GRANT readwrite TO usuario2;
