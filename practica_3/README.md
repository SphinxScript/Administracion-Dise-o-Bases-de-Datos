## Entidades
### 1. Vivero:  Representa los centros físicos de venta y producción de plantas y jardinería pertenecientes a la empresa Tajinaste S.A.
### 2. Zona: Representa las divisiones y áreas físicas dentro de un vivero concreto (por ejemplo, zona exterior, almacén principal, invernadero).
### 3. Producto: Contempla todos los artículos que comercializa Tajinaste S.A.
### 4. Tarea: Define las actividades u operaciones específicas que deben desempeñarse dentro de los puestos de trabajo de la empresa.
### 5. Puesto: Representa el cargo o función laboral asignado a los empleados dentro de la estructura organizativa.
### 6. Empleado: Corresponde al personal contratado por Tajinaste S.A. que desempeña funciones en los viveros y gestiona la venta de productos. 
### 7. Pedido: Registra cada transacción comercial u orden de compra procesada para los clientes pertenecientes al programa de fidelización. 
### 8. Cliente Tajinaste Plus: Almacena la información de los clientes inscritos en el programa de fidelización "Tajinaste Plus". 
### 9. Bonificación: Representa el cálculo de la recompensa mensual asignada a un cliente Tajinaste Plus según el acumulado de sus compras.


## 2. Dominio y Ejemplos Ilustrativos de los Atributos

### Atributos de Entidades

    Viveros

        Id_vivero: Cadena alfanumérica única (PK) | Ejemplo: "VIV-001"

        Nombre: Texto (longitud máx. 100) | Ejemplo: "Vivero Anaga Central"

        Latitud: Coma flotante / Decimal [-90, 90] | Ejemplo: 28.46824

        Longitud: Coma flotante / Decimal [-180, 180] | Ejemplo: -16.25462

    Zona

        Id_zona: Cadena alfanumérica única (PK) | Ejemplo: "ZON-012"

        Nombre: Texto (longitud máx. 50) | Ejemplo: "Invernadero A - Plantas de Sombra"

        Latitud: Coma flotante / Decimal [-90, 90] | Ejemplo: 28.46830

        Longitud: Coma flotante / Decimal [-180, 180] | Ejemplo: -16.25470

    Producto

        Id_producto: Cadena alfanumérica única (PK) | Ejemplo: "PROD-104"

        Nombre: Texto (longitud máx. 100) | Ejemplo: "Tajinaste Rojo 3L"

        Descripción: Texto multilínea | Ejemplo: "Especie autóctona en maceta de 3 litros"

        Precio_base: Moneda / Real decimal >= 0 | Ejemplo: 18.50 €

    Tarea

        Id_tarea: Cadena alfanumérica única (PK) | Ejemplo: "TAR-003"

        Nombre_tarea: Texto (longitud máx. 50) | Ejemplo: "Supervisión de Riego Automático"

        Descripción: Texto explicativo | Ejemplo: "Mantenimiento y programación de aspersores"

    Puesto

        Id_puesto: Cadena alfanumérica única (PK) | Ejemplo: "PUE-002"

        Nombre_puesto: Texto (longitud máx. 50) | Ejemplo: "Encargado de Botánica"

        Descripción: Texto descriptivo | Ejemplo: "Coordinación del cuidado de flora delicada"

    Empleado

        Id_empleado: Cadena alfanumérica única (PK) | Ejemplo: "EMP-045"

        DNI: Cadena de 8 dígitos y 1 letra | Ejemplo: "43812345X"

        Nombre: Texto (longitud máx. 50) | Ejemplo: "Laura"

        Apellidos: Texto (longitud máx. 100) | Ejemplo: "García Pérez"

        Número_Teléfono: Cadena numérica de 9 dígitos | Ejemplo: "612345678"

    Pedido

        Id_pedido: Cadena alfanumérica única (PK) | Ejemplo: "PED-2026-089"

        Fecha: Timestamp (Fecha y Hora) | Ejemplo: "2026-05-10 11:30:00"

        Coste_total: Moneda / Real decimal >= 0 | Ejemplo: 142.30 €

    Cliente Tajinaste Plus

        Id_cliente: Cadena alfanumérica única (PK) | Ejemplo: "CLI-0012"

        Nombre: Texto (longitud máx. 50) | Ejemplo: "Carlos"

        Apellidos: Texto (longitud máx. 100) | Ejemplo: "Rodríguez Martín"

        Email: Dirección de correo electrónica válida | Ejemplo: "carlos.rm@email.com"

        Fecha_ingreso: Fecha (AAAA-MM-DD) | Ejemplo: "2024-01-15"

    Bonificación

        Id_bonificacion: Cadena alfanumérica única (PK) | Ejemplo: "BON-202604-012"

        Año: Entero positivo [1900, 2100] | Ejemplo: 2026

        Mes: Entero [1, 12] | Ejemplo: 4 (Abril)

        Total_bonificado: Moneda / Real decimal >= 0 | Ejemplo: 25.00 €

### Atributos de Relaciones

    Almacena

        Cantidad: Entero positivo >= 0 | Ejemplo: 120 (unidades almacenadas en esa zona)

    Asignado

        Fecha_inicio: Fecha (AAAA-MM-DD) | Ejemplo: "2026-01-01"

        Fecha_fin: Fecha (AAAA-MM-DD) o Nulo si está activo | Ejemplo: "2026-06-30"

    Contiene

        Cantidad: Entero positivo >= 1 | Ejemplo: 3 (unidades compradas en la orden)

        Precio: Moneda / Real decimal >= 0 (precio de venta al momento) | Ejemplo: 18.50 €
---

## 3. Descripción Detallada de Relaciones y Cardinalidades

### 1. Tiene (`Vivero` — `Zona`)
* **Descripción:** Relación de composición territorial que subdivide un vivero en distintas zonas georreferenciadas.
* **Cardinalidad `Vivero` (1,N):** Un vivero debe tener al menos una zona definida (por ejemplo, almacén) y puede tener múltiples zonas distintas.
* **Cardinalidad `Zona` (1,1):** Cada zona pertenece única y exclusivamente a un vivero específico.

### 2. Almacena (`Zona` — `Producto`)
* **Descripción:** Controla el inventario de existencias (stock) de los distintos productos disponibles en cada zona física.
* **Cardinalidad `Zona` (0,N):** Una zona puede no contener existencias de ningún producto en un momento dado (0) o almacenar múltiples productos (N).
* **Cardinalidad `Producto` (0,N):** Un producto puede no estar disponible actualmente en ninguna zona (0) o encontrarse distribuido en múltiples zonas de los viveros (N).

### 3. Realiza (`Puesto` — `Tarea`)
* **Descripción:** Define el catálogo de responsabilidades y actividades que componen las competencias de un puesto de trabajo.
* **Cardinalidad `Puesto` (1,N):** Todo puesto de trabajo tiene asociada al menos una tarea obligatoria (1) y puede abarcar múltiples tareas (N).
* **Cardinalidad `Tarea` (1,N):** Una tarea específica puede ser compartida y realizada por uno (1) o varios puestos de trabajo (N).

### 4. Gestiona (`Empleado` — `Pedido`)
* **Descripción:** Asigna la responsabilidad del seguimiento de la venta y consecución de objetivos a un empleado para los pedidos efectuados por los clientes Tajinaste Plus.
* **Cardinalidad `Empleado` (0,N):** Un empleado puede no haber gestionado ningún pedido (0, por ejemplo, si no es vendedor o es de reciente incorporación) o ser responsable de la gestión de múltiples pedidos (N).
* **Cardinalidad `Pedido` (1,1):** Cada pedido tramitado debe contar obligatoriamente con un único empleado responsable (1,1).

### 5. Contiene (`Pedido` — `Producto`)
* **Descripción:** Especifica la composición de artículos y cantidades que integran una orden de compra concreta.
* **Cardinalidad `Pedido` (1,N):** Un pedido debe incluir como mínimo un producto registrado (1) y puede estar compuesto por múltiples productos (N).
* **Cardinalidad `Producto` (0,N):** Un producto del catálogo puede no haber sido comprado aún en ningún pedido (0) o estar presente en múltiples pedidos (N).

### 6. Realiza (`Cliente Tajinaste Plus` — `Pedido`)
* **Descripción:** Registra las compras efectuadas por cada uno de los clientes adscritos al programa de fidelización.
* **Cardinalidad `Cliente Tajinaste Plus` (0,N):** Un cliente afiliado puede no haber realizado ningún pedido aún desde su registro (0) o acumular un número indeterminado de pedidos (N).
* **Cardinalidad `Pedido` (1,1):** Todo pedido realizado dentro del programa pertenece a un único cliente registrado (1,1).

### 7. Obtiene (`Cliente Tajinaste Plus` — `Bonificación`)
* **Descripción:** Relaciona los incentivos económicos liquidados mensualmente a favor de un cliente según sus volúmenes de compra acumulados.
* **Cardinalidad `Cliente Tajinaste Plus` (0,N):** Un cliente puede no haber alcanzado los umbrales de bonificación en ningún mes (0) o haber obtenido múltiples bonificaciones a lo largo de su membresía (N).
* **Cardinalidad `Bonificación` (1,1):** Cada registro de bonificación mensual es nominal y pertenece a un único cliente (1,1).

---

## 4. Restricciones Semánticas (Reglas de Negocio)

1. **Unicidad de Destino (No Solapamiento Temporal):** Un empleado no puede tener más de una asignación activa simultáneamente. Formalmente, para un mismo `Id_empleado`, no pueden existir dos registros en la relación `Asignado` donde los intervalos [`Fecha_inicio`, `Fecha_fin`] se solapen en el tiempo. Si `Fecha_fin` es nula, se asume que la asignación sigue activa.
2. **Consistencia Territorial en Pedidos:** El empleado asignado como responsable en la relación `Gestiona` de un pedido debe haber estado destinado en la `Zona` o `Vivero` donde se procesó la venta en la fecha exacta en la que se generó el pedido.
3. **Unicidad Mensual de Bonificación:** Para una combinación dada de `Año`, `Mes` e `Id_cliente`, solo puede existir un registro único en la entidad `Bonificación`.
4. **Congelación del Precio Histórico:** El atributo `Precio` de la relación `Contiene` se fija en el instante de tramitación del pedido y no variará aunque sufra modificaciones el atributo `Precio_base` de la entidad `Producto`.
5. **Validación de Intervalos Fechas:** En la relación `Asignado`, siempre se debe cumplir la restricción semántica de que `Fecha_fin` > `Fecha_inicio`.