# JARDINERIA

## DESCRIPCION

En este repositorio se encuentra la base de datos sobre jardineria y el readme con sus consultas.

## MODELO FISICO

![Modelo físico jardineria](./jardineria.png)

## CONSULTAS JARDINERIA

### 1.4.5 Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_representante_ventas
    FROM cliente c
    JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
    ORDER BY nombre_cliente ASC;
    ```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_representante_ventas
    FROM cliente c
    JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    JOIN empleado e ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    ORDER BY nombre_cliente ASC;
    ```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_representante_ventas
    FROM cliente c
    LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    JOIN empleado e ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    WHERE p.codigo_cliente IS NULL
    ORDER BY nombre_cliente ASC;
    ```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_representante_ventas, CONCAT(o.ciudad) AS ciudad_oficina
    FROM cliente c
    JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
    JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    ORDER BY nombre_cliente ASC;
    ```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_representante_ventas, CONCAT(o.ciudad) AS ciudad_oficina
    FROM cliente c
    LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
    JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    WHERE p.codigo_cliente IS NULL
    ORDER BY nombre_cliente ASC;
    ```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

    ```sql
    SELECT DISTINCT CONCAT('Principal: ', o.linea_direccion1, ' Complemento: ', o.linea_direccion2) AS Dirección_Oficina
    FROM oficina o
    JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
    JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    WHERE c.ciudad = "Fuenlabrada"
    ORDER BY Dirección_Oficina ASC;
    ```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_representante, CONCAT(o.ciudad) AS ciudad_oficina
    FROM cliente c
    JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
    JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    ORDER BY nombre_cliente ASC;
    ```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

    ```sql
    SELECT CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_empleado, CONCAT(e2.nombre, ' ', e2.apellido1, ' ', e2.apellido2) AS nombre_jefe
    FROM empleado e
    JOIN empleado e2 ON e.codigo_jefe = e2.codigo_empleado
    ORDER BY nombre_empleado ASC;
    ```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

    ```sql
    SELECT CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS nombre_empleado, CONCAT(e2.nombre, ' ', e2.apellido1, ' ', e2.apellido2) AS nombre_jefe, CONCAT(e3.nombre, ' ', e3.apellido1, ' ', e3.apellido2) AS nombre_jefe_del_jefe
    FROM empleado e
    JOIN empleado e2 ON e.codigo_jefe = e2.codigo_empleado
    JOIN empleado e3 ON e2.codigo_jefe = e3.codigo_empleado
    ORDER BY nombre_empleado ASC;
    ```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

    ```sql
    SELECT c.nombre_cliente AS nombre_cliente
    FROM cliente c
    JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    WHERE fecha_entrega > fecha_esperada OR fecha_entrega IS NULL
    ORDER BY nombre_cliente ASC;
    ```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

    ```sql
    SELECT c.codigo_cliente, GROUP_CONCAT(DISTINCT pr.gama) AS gama_producto
    FROM cliente c
    JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido
    JOIN producto pr ON d.codigo_producto = pr.codigo_producto
    GROUP BY codigo_cliente
    ORDER BY codigo_cliente;
    ```

### 1.4.6 Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

    ```sql
    SELECT c.*
    FROM cliente c
    LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    WHERE p.codigo_cliente IS NULL
    ORDER BY c.nombre_cliente;
    ```

2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.

    ```sql
    SELECT c.*
    FROM cliente c
    LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    WHERE p.codigo_cliente IS NULL
    ORDER BY c.nombre_cliente;
    ```

3. Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.

    ```sql
    SELECT c.*
    FROM cliente c
    LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    LEFT JOIN pedido pe ON c.codigo_cliente = pe.codigo_cliente
    WHERE p.codigo_cliente IS NULL OR pe.codigo_cliente IS NULL
    ORDER BY c.nombre_cliente;
    ```

4. Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.

    ```sql
    SELECT e.*
    FROM empleado e
    LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    WHERE o.codigo_oficina IS NULL
    ORDER BY e.nombre;
    ```

5. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.

    ```sql
    SELECT e.*
    FROM empleado e
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    WHERE c.codigo_empleado_rep_ventas IS NULL
    ORDER BY e.nombre;
    ```

6. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la oficina donde trabajan.

    ```sql
    SELECT e.*, o.*
    FROM empleado e
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    WHERE c.codigo_empleado_rep_ventas IS NULL
    ORDER BY e.nombre;
    ```

7. Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

    ```sql
    SELECT e.*
    FROM empleado e
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    WHERE c.codigo_empleado_rep_ventas IS NULL OR o.codigo_oficina IS NULL
    ORDER BY e.nombre;
    ```

8. Devuelve un listado de los productos que nunca han aparecido en un pedido.

    ```sql
    SELECT DISTINCT p.*
    FROM producto p
    LEFT JOIN detalle_pedido d ON p.codigo_producto = d.codigo_producto
    WHERE d.codigo_producto IS NULL
    ORDER BY p.nombre;
    ```

9. Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.

    ```sql
    SELECT DISTINCT p.nombre AS nombre_producto, p.descripcion AS descripcion_producto, g.imagen AS imagen_producto
    FROM producto p
    LEFT JOIN detalle_pedido d ON p.codigo_producto = d.codigo_producto
    LEFT JOIN gama_producto g ON p.gama = g.gama
    WHERE d.codigo_producto IS NULL
    ORDER BY p.nombre;
    ```

10. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

    ```sql
    SELECT DISTINCT o.*
    FROM oficina o
    JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
    JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido
    JOIN producto pr ON d.codigo_producto = pr.codigo_producto
    WHERE pr.gama != 'Frutales';
    ```

11. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

    ```sql
    SELECT DISTINCT c.*
    FROM cliente c
    JOIN pedido pe ON c.codigo_cliente = pe.codigo_cliente
    LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    WHERE p.codigo_cliente IS NULL
    ORDER BY c.nombre_cliente;
    ```

12. Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.

    ```sql
    SELECT DISTINCT e.*, CONCAT(e2.nombre, ' ', e2.apellido1, ' ', e2.apellido2) AS nombre_jefe
    FROM empleado e
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
    LEFT JOIN empleado e2 ON e.codigo_jefe = e2.codigo_empleado
    WHERE c.codigo_empleado_rep_ventas IS NULL
    ORDER BY e.nombre;
    ```