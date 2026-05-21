# LMSGI_UD07_Espejo_Martinez_Jorge
Aquí se desarrollará la UD07_03_SistemasGestionEmpresarialAEE donde la empresa andaluza de servicios tecnológicos "WillmanTech S.L." acaba de finalizar la implantación de su infraestructura ERP/CRM para optimizar sus flujos de ventas y facturación.

Estructura:
LMSGI_UD07_Espejo_Martinez_Jorge  
  ├── report\_invoice\_willmantech.xml  
  ├── interoperabilidad/  
  │   ├── invoice\_export.json           
  │   └── invoice\_ubl.xml               
  └── manual\_explotacion\_willmantech.md (README.md)

# manual_explotacion_willmantech.md

## I. Generación del Informe Dinámico (QWeb XML)

Se debe diseñar y programar la estructura XML de una plantilla de informe de factura personalizada para "WillmanTech S.L." utilizando la sintaxis de **QWeb**.

Requisitos mínimos del archivo report\_invoice\_willmantech.xml:

1. **Sintaxis estructurada:** El documento debe estar bien formado e incluir espacios de nombres correctos si fuera necesario.  
2. **Lógica embebida QWeb:** Debe hacer uso de directivas de iteración (t-foreach) para desglosar dinámicamente las líneas de la factura (invoice\_line\_ids).  
3. **Lógica condicional:** Debe implementar una condición (t-if) para ocultar la columna de "Descuento" en caso de que ninguna de las líneas de detalle de la factura lo contenga.  
4. **Data Binding:** Utilizar la directiva t-field para mostrar de forma limpia campos como el número de factura (doc.name), fecha de emisión (doc.date) y el total neto (doc.amount\_total).

## II. Interoperabilidad de Datos (Extracción JSON/XML)

Para permitir que las facturas de "**WillmanTech S.L.**" se incorporen a la plataforma de la Agencia Tributaria o se sincronicen con sistemas contables externos de manera asíncrona, se debe definir las estructuras de intercambio de datos. Por ello se necesita:

1. **invoice\_export.json:** Un archivo que contenga un array estructurado con la información de una factura de ejemplo recuperada dinámicamente mediante una consulta al ERP. Debe contener campos relacionales anidados (ID de cliente, nombre, email, líneas de producto, precios y base imponible).  
2. **invoice\_ubl.xml:** Un fragmento de factura electrónica simplificado que cumpla con las especificaciones del estándar internacional **UBL (Universal Business Language)**, incluyendo obligatoriamente los namespaces de componentes agregados (cac) y componentes básicos (cbc), así como el ID de personalización europeo compatible con la red PEPPOL.

## III. Manual de Explotación bajo Norma ISO/IEC/IEEE 26514

Este es el documento técnico que servirá de guía para los administradores de sistemas y usuarios finales que exploten el ERP de "**WillmanTech S.L.**" Este documento se estructurará siguiendo los requisitos de calidad y usabilidad del estándar internacional **ISO/IEC/IEEE 26514:2022**.

El manual (manual\_explotacion\_willmantech.md o README.md) incluye las siguientes secciones:

### 1. **Introducción y Arquitectura:**
Módulos activados en el ERP, son:  
- Ventas (Sales)  
- Facturación (Invoicing/Accounting)  
- Inventario (Inventory)  
Topología lógica  
El sistema se despliega mediante Docker Compose con dos contenedores principales: un servidor Odoo (puerto 8200) y la  base de datos de Odoo (puerto 8069).
### 2. **Guía de Instalación y Reinstalación:**
Requisitos previos:  
Docker y Docker Compose instalados  
Git (para clonar el repositorio)  
4GB RAM mínimo, 8GB recomendado  

Pasos detallados:  
1. Clonar el repositorio
git clone https://github.com/enlace.git
cd odoo-docker

2. Crear archivo .env con variables de entorno
cat > .env << EOF
POSTGRES_DB=willmantech
POSTGRES_USER=odoo
POSTGRES_PASSWORD=contraseña
ODOO_ADMIN_PASSWORD=admin_password
EOF

3. Levantar contenedores
docker-compose up -d

4. Verificacion
docker-compose ps
docker-compose logs odoo

Variables de entorno necesarias:  
POSTGRES_DB: Nombre de la base de datos
POSTGRES_USER: Usuario de PostgreSQL
POSTGRES_PASSWORD: Contraseña
ODOO_ADMIN_PASSWORD: Contraseña Admin

Dependencias del SGBD:  
PostgreSQL 15 o superior
Extensión uuid-ossp habilitada

Reinstalación: detener los contenedores, eliminar los volúmenes (docker-compose down -v) y repetir los pasos.
### 3. **Seguridad y Control de Acceso:**
Configuración de roles (administrador, contable, comercial), políticas de contraseñas y privilegios sobre la información.
| **Rol**   | **Permisos** | **Módulos** |
|-----------|--------------|-------------|
| Admin     | Acceso completo a todos los módulos y configuración del sistema | Todos |
| Contable  | Facturación completa, pagos, informes contables | Facturación, Contabilidad |
| Comercial | Crear/editar presupuestos, confirmar pedidos, solo lectura en facturación | Ventas |
| Almacén   | Gestión de entradas/salidas de inventario | Inventario |
| Usuario   | Solo consulta de facturas y pedidos | Portal |  

Cómo configurar los roles en Odoo:  
Ir a Ajustes → Usuarios y Compañías → Usuarios
Crear nuevo usuario o editar existente
En la pestaña "Derechos de acceso", asignar los grupos correspondientes 
Los administradores tienen el grupo "Ajustes" habilitado

Políticas de contraseñas:  
Longitud mínima: 8 caracteres
Requiere mayúsculas, minúsculas, números y símbolos
Caducidad: 90 días
Historial: no repetir las últimas 5 contraseñas

Privilegios sobre la información:  
Reglas por registro: los comerciales solo ven sus propios clientes 
Los contables ven todas las facturas pero no pueden modificar pedidos de venta 
### 4. **Procedimiento de Backup y Restauración:**
Detalle del comando para respaldar la base de datos relacional y los almacenes de datos asociados.
### 5. **Flujo Operativo de Facturación e Informes:**
Explicación paso a paso de cómo un usuario genera una factura en la interfaz y cómo el sistema renderiza el informe final a PDF (explicando de manera didáctica el pipeline HTML \-\> wkhtmltopdf \-\> PDF).
1. Ir a la aplicación Facturación
2. Crear nueva factura (Crear → Factura)
3. Seleccionar cliente
4. Añadir líneas de producto (descripción, cantidad, precio)
5. Validar la factura (botón "Confirmar")
6. Hacer clic en Imprimir → Factura
