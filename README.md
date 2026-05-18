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

## Generación del Informe Dinámico (QWeb XML)

Se debe diseñar y programar la estructura XML de una plantilla de informe de factura personalizada para "WillmanTech S.L." utilizando la sintaxis de **QWeb**.

Requisitos mínimos del archivo report\_invoice\_willmantech.xml:

1. **Sintaxis estructurada:** El documento debe estar bien formado e incluir espacios de nombres correctos si fuera necesario.  
2. **Lógica embebida QWeb:** Debe hacer uso de directivas de iteración (t-foreach) para desglosar dinámicamente las líneas de la factura (invoice\_line\_ids).  
3. **Lógica condicional:** Debe implementar una condición (t-if) para ocultar la columna de "Descuento" en caso de que ninguna de las líneas de detalle de la factura lo contenga.  
4. **Data Binding:** Utilizar la directiva t-field para mostrar de forma limpia campos como el número de factura (doc.name), fecha de emisión (doc.date) y el total neto (doc.amount\_total).

## Interoperabilidad de Datos (Extracción JSON/XML)

Para permitir que las facturas de "**WillmanTech S.L.**" se incorporen a la plataforma de la Agencia Tributaria o se sincronicen con sistemas contables externos de manera asíncrona, se debe definir las estructuras de intercambio de datos. Por ello se necesita:

1. **invoice\_export.json:** Un archivo que contenga un array estructurado con la información de una factura de ejemplo recuperada dinámicamente mediante una consulta al ERP. Debe contener campos relacionales anidados (ID de cliente, nombre, email, líneas de producto, precios y base imponible).  
2. **invoice\_ubl.xml:** Un fragmento de factura electrónica simplificado que cumpla con las especificaciones del estándar internacional **UBL (Universal Business Language)**, incluyendo obligatoriamente los namespaces de componentes agregados (cac) y componentes básicos (cbc), así como el ID de personalización europeo compatible con la red PEPPOL.

## Manual de Explotación bajo Norma ISO/IEC/IEEE 26514

Este es el documento técnico que servirá de guía para los administradores de sistemas y usuarios finales que exploten el ERP de "**WillmanTech S.L.**" Este documento se estructurará siguiendo los requisitos de calidad y usabilidad del estándar internacional **ISO/IEC/IEEE 26514:2022**.

El manual (manual\_explotacion\_willmantech.md o README.md) incluye las siguientes secciones:

### 1. **Introducción y Arquitectura:**
Descripción técnica de los módulos activados en el ERP, indicando la topología lógica (ej. despliegue mediante Docker Compose).
### 2. **Guía de Instalación y Reinstalación:**
Pasos detallados para levantar el entorno desde cero, indicando variables de entorno necesarias y dependencias del SGBD.
### 3. **Seguridad y Control de Acceso:**
Configuración de roles (administrador, contable, comercial), políticas de contraseñas y privilegios sobre la información.
### 4. **Procedimiento de Backup y Restauración:**
Detalle del comando para respaldar la base de datos relacional y los almacenes de datos asociados.
### 5. **Flujo Operativo de Facturación e Informes:**
Explicación paso a paso de cómo un usuario genera una factura en la interfaz y cómo el sistema renderiza el informe final a PDF (explicando de manera didáctica el pipeline HTML \-\> wkhtmltopdf \-\> PDF).