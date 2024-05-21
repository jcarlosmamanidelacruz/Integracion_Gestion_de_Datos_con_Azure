# Solución de Integración y Gestión de Datos en la Nube con Azure

## Descripción del Proyecto

En este proyecto, se ha desplegado una solución completa de análisis de datos utilizando Microsoft Azure. El proceso abarcó la creación y configuración de varios servicios de Azure, incluyendo Azure Synapse Analytics, Azure Data Factory, Azure SQL Database, Azure Key Vault, y Azure Databricks. A continuación, se presentan las conclusiones clave de cada componente y las mejores prácticas observadas:

## Azure Synapse Analytics

- Plataforma Unificada: Azure Synapse Analytics proporciona una plataforma integrada para la ingestión, preparación, manejo y análisis de datos a gran escala.

- Integración con Data Lake: La integración con Azure Data Lake Storage facilita la gestión y procesamiento de grandes volúmenes de datos.

- Seguridad: Es crucial configurar la seguridad y autenticación adecuadamente para proteger los datos y asegurar el acceso adecuado.

## Azure Data Factory

- Orquestación de Flujos de Datos: Fundamental para la orquestación de flujos de datos, permitiendo la integración y transformación de datos de diversas fuentes.

- Linked Services y Datasets: La creación de Linked Services y Datasets facilita la conexión a diferentes fuentes de datos y la realización de operaciones ETL (Extract, Transform, Load).

## Azure SQL DatabaseAzure SQL Database

- Base de Datos Relacional: Proporciona una base de datos relacional altamente disponible y escalable.

- Seguridad: La configuración del firewall y la gestión de reglas de acceso son esenciales para asegurar la base de datos y permitir el acceso autorizado.

## Azure Key Vault

- Gestión de Secretos: Es una herramienta esencial para la gestión segura de secretos y claves de encriptación.

- Integración: La integración con otros servicios de Azure mediante Managed Identities simplifica la gestión de credenciales y mejora la seguridad.

## Azure Databricks

- Procesamiento de Datos: Ofrece una plataforma robusta para el procesamiento y análisis de datos mediante Apache Spark.

- Organización en Capas: La creación de clusters y la ejecución de notebooks permiten realizar operaciones de ETL y análisis de datos de manera eficiente. La organización en capas (landing, bronze, silver, gold) utilizando Delta Lake mejora la gestión y el rendimiento de los datos.

## Integración y Orquestación

- Solución End-to-End: La integración de todos estos servicios permite construir una solución de análisis de datos end-to-end que es escalable, segura y eficiente.

- Gestión de Permisos: La configuración adecuada de los permisos y la gestión de identidades es fundamental para asegurar que cada componente pueda interactuar correctamente y de manera segura.

## Replicación del Proyecto

Este proyecto está diseñado paso a paso para que cualquier persona pueda replicarlo siguiendo la documentación detallada disponible en este repositorio. Cada paso, desde la creación de los servicios de Azure hasta la configuración y ejecución de los flujos de datos, se documenta exhaustivamente para facilitar la reproducción del proyecto en cualquier entorno Azure.

## Conclusiones y Mejores Prácticas

Este proyecto demuestra cómo los servicios de Azure pueden integrarse de manera efectiva para crear una solución robusta de análisis de datos. Siguiendo las mejores prácticas de seguridad, automatización y escalabilidad, es posible construir una infraestructura eficiente y segura que soporte el análisis de grandes volúmenes de datos.

## Mejores Prácticas

- Seguridad: Implementar políticas de seguridad estrictas y utilizar servicios como Azure Key Vault para la gestión de secretos asegura que los datos estén protegidos en todo momento.

- Escalabilidad: Aprovechar las capacidades de escalabilidad de Azure permite manejar grandes volúmenes de datos y ajustar los recursos según las necesidades del proyecto.

- Monitoreo y Mantenimiento: Configurar alertas y monitoreo continuo de los servicios permite identificar y resolver problemas rápidamente, garantizando la alta disponibilidad y rendimiento del sistema.

## Recursos Adicionales

Para obtener más información sobre los servicios utilizados en este proyecto, consulta los siguientes recursos:

[Documentación de Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/ "Documentación de Azure Synapse Analytics")

[Guía de Azure Data Factory](https://learn.microsoft.com/en-us/azure/data-factory/ "Guía de Azure Data Factory")

[Documentación de Azure SQL Database](https://learn.microsoft.com/en-us/azure/azure-sql/?view=azuresql "Documentación de Azure SQL Database")

[Tutorial de Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/ "Tutorial de Azure Key Vault")

[Documentación de Azure Databricks](https://learn.microsoft.com/en-us/azure/databricks/ "Documentación de Azure Databricks")

# CREACIÓN SYNAPSE ANALYTICS

1 . Iniciar Sesión en Azure Portal y crear grupo de recursos

**Nombre grupo de recursos:** rg-datapath-synapse-002
**Region**: (US) East US 2

[![crear-grupo-recursos.png](https://i.postimg.cc/cC4xSXX0/crear-grupo-recursos.png)](https://postimg.cc/YGZKNN5s)

2 . Ingresar al grupo de recursos creado, y en la parte superior seleccionar “Create"

[![2-ingresar-RG.png](https://i.postimg.cc/L4T7gH6y/2-ingresar-RG.png)](https://postimg.cc/NLKbWvXT)

3 . En el Marketplace escribir “azure synapse analytics” y seleccionar el recurso, luego clic en “create”

[![3-create-Synaps-Analytics.png](https://i.postimg.cc/d0sZ1mWN/3-create-Synaps-Analytics.png)](https://postimg.cc/YvPCDgBQ)

4 . Configurar el workspace en “basics”:

a. Manage resource group: rg-manage-synapse-jc
b. Workspace name: synw-serverless-jc (nombre debe ser único)
c. Region: East US

[![4-configuracion-workspace.png](https://i.postimg.cc/PqnNcGvw/4-configuracion-workspace.png)](https://postimg.cc/3k9K4cB8)

Creamos un nuevo Azure Data lake Storage

d. Account name: clic en “create new” → adlssynapseey02 (nombre debe ser único)
e. File system name: clic en “create new” → source

[![5-creacion-datalake-storage.png](https://i.postimg.cc/GhBFnCt7/5-creacion-datalake-storage.png)](https://postimg.cc/N5c9RZ1m)

5 . Configurar el workspace en “Security”:

a. Authentication method: Use both local and Azure Active Directory (Azure AD) authentication
b. SQL Server admin login: sqladminuser
c. Escribir y confirmar contraseña
	
[![6-configuracion-security.png](https://i.postimg.cc/R06MRgZD/6-configuracion-security.png)](https://postimg.cc/XZWSNcFf)

6 . Clic en “review + create” → Create

Nota: Al momento de crearSsynapse, por defecto nos va a crear el Synapse serverless pool.

# CREACIÓN AZURE DATA FACTORYCREACIÓN AZURE DATA FACTORY

1.- Ir al grupo de recursos creado "rg-datapath-synapse-002", luego ir a “+ Create”, finalmente en el Marketplace escribir “azure data factory” y seleccionar “Create”

[![7-crear-azure-DF.png](https://i.postimg.cc/9MNgpHJ6/7-crear-azure-DF.png)](https://postimg.cc/HcXtYRGz)

2.- En la sección “Basics” configurar lo siguiente:

a. Resource group: rg-datapath-synapse-002
b. Name: adf-emissions-jc
c. Region: East US 2
d. Version: V2

[![8-configuracion-DF.png](https://i.postimg.cc/xCJjgn9H/8-configuracion-DF.png)](https://postimg.cc/xJQS1rJT)

Clic en “Review + Create” —> “Create” —> “Go to resource”

3.- Dentro del recurso creado clic en “Launch studio”

[![9-DF-creado.png](https://i.postimg.cc/K8DdXV0W/9-DF-creado.png)](https://postimg.cc/jDC8P806)

# CREACIÓN AZURE SQL DATABASE

1.- Ir al grupo de recursos creado "rg-datapath-synapse-002", luego ir a “+ Create”, finalmente en el Marketplace escribir “sql database” y seleccionar “Create”

[![10-create-SQL-database.png](https://i.postimg.cc/vTpcsDZG/10-create-SQL-database.png)](https://postimg.cc/m1yb3L9n)

2.- En la sección “Basics” configurar lo siguiente:
a. Resource group: rg-datapath-synapse-001
b. Database Name: db-datapath
c. Server: “Create New”

[![11-create-SQL-database-configuracion.png](https://i.postimg.cc/9XgsPKMP/11-create-SQL-database-configuracion.png)](https://postimg.cc/xqHsnsLq)

C.1 “Create New”

a. Server name: server-datapath
b. Location: (US) East US 2
c. Authentication method: Use SQL authentication
d. Server admin login: username

[![12-create-SQL-database-server.png](https://i.postimg.cc/9fNNPqXm/12-create-SQL-database-server.png)](https://postimg.cc/kVKcqGtz)

Clic en “OK”

d. Want to use SQL elastic pool?: No
e. Workload environment: Development
f. Backup storage: Local

[![13-Sql-elasic-pool.png](https://i.postimg.cc/Qx44ZT5p/13-Sql-elasic-pool.png)](https://postimg.cc/vcnLWD0B)

Clic en “Review + Create” —> “Create” —> “Go to resource”

3.- Ir a "query editor (preview)”

Ingresar con las credenciales, nos aparecerá un error debido al firewall

[![14-Sql-elasic-pool.png](https://i.postimg.cc/MphbDb2p/14-Sql-elasic-pool.png)](https://postimg.cc/87m6kvz2)

4.- Ir a “Overview” luego en la parte superior central ir a “ Set server firewall” →“Public access” —> “select networks”

[![15-selected-networks.png](https://i.postimg.cc/yNPZgwKc/15-selected-networks.png)](https://postimg.cc/B8j6R7Xn)

5.- Ir a la sección de “Firewall rules” luego clic en “ Add a firewall rule” a
continuación colocar nuestra ip de conexión a internet, finalmente “save”

[![16-add-ip-rules.png](https://i.postimg.cc/Qd9pnX7b/16-add-ip-rules.png)](https://postimg.cc/Hrmcr1Bc)

6.- Ir a “Query editor” ingresar con nuestras credenciales
7.- Abrir un “New Query” y pegar la siguiente consulta:

		CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL,
			FirstName varchar(50),
			LastName varchar(50)
		)
		GO
		
7-. Ir a nuestro y dentro de nuestro contenedor “source” crear una carpeta llamada "input" finalmente subir archivo “emp.txt”

# CREACIÓN AZURE KEY VAULT

1.- Ir al grupo de recursos creado "rg-datapath-synapse-002", luego ir a “+ Create”, finalmente en el Marketplace escribir “key vault” y seleccionar “Create"

[![17-create-key-vault.png](https://i.postimg.cc/nzHpmqqh/17-create-key-vault.png)](https://postimg.cc/McrC2MsC)

2.- En la sección “Basics” configurar lo siguiente:

a. Resource group: rg-datapath-synapse-002
b. Key vault Name: kv-datapath-jc
c. Region: East US 2
d. Pricing Tier: Standard

[![18-create-key-vault-optons.png](https://i.postimg.cc/xdVbWFVp/18-create-key-vault-optons.png)](https://postimg.cc/VJ46tg6X)

Clic en “Review + Create” —> “Create” —> “Go to resource”

# OTORGAR PERMISOS

1.- Ir a “Access control (IAM), luego “Add”, clic en “Add role assignment”

[![19-add-roles.png](https://i.postimg.cc/Vs04Qpxm/19-add-roles.png)](https://postimg.cc/XX399HNz)

2.- En “Job function roles” seleccionar “key vault contributor”, luego clic en“Members”

[![20-add-roles-member.png](https://i.postimg.cc/W33Wk4Yk/20-add-roles-member.png)](https://postimg.cc/Y45zctHr)

3.- En “Assign access to” seleccionar ‘User, group, or service principal’, luego en “Members” clic en ‘+ Select members’

[![21-add-roles-member-select.png](https://i.postimg.cc/zGDpHBcF/21-add-roles-member-select.png)](https://postimg.cc/rKb5v8cD)

4.- En la ventana abierta escribir nuestro usuario y selecionarlo

[![22-select-member.png](https://i.postimg.cc/qqVHpnQL/22-select-member.png)](https://postimg.cc/vgzNz4Jg)

Clic en “select” —> “Review + assign”

5.- Repetimos los pasos para asignar permisos a nuestro servicio Azure Data
Factory, cambiando en el paso 3 “Assign access” a ‘Manage identity’

[![23-select-member-managed-identity.png](https://i.postimg.cc/mgJRgtLt/23-select-member-managed-identity.png)](https://postimg.cc/sMYbnfZr)

6.- Seleccionar nuestro servicio, finalmente clic en “select”

[![24-select-member-managed-identity-assign.png](https://i.postimg.cc/BvPFjL2y/24-select-member-managed-identity-assign.png)](https://postimg.cc/TyG1SP7q)

Doble clic en “Review + assign”

7.- Los permisos deberán aparecer así:

[![25-permisos-finales.png](https://i.postimg.cc/TYXvSZkW/25-permisos-finales.png)](https://postimg.cc/MXtLfrJW)

# CREACIÓN LINKED SERVICES ADF

1.- Ir al grupo al ADF Studio, dar clic en “Manage” en la sección de
connections clic en “Linked services” finalmente clic en “New”

[![26-linked-services.png](https://i.postimg.cc/3JLJkVP6/26-linked-services.png)](https://postimg.cc/WhkPKWp6)

2.- En la sección “New linked service” buscar y dar clic en ‘Azure Data Lake Storage Gen 2’, finalmente clic en “continue”

[![27-azure-data-lake.png](https://i.postimg.cc/6QqH92Cg/27-azure-data-lake.png)](https://postimg.cc/4KCvP3LQ)

3.- En la ventana emergente configurar lo siguiente:

a. Name: lkdsADLS
b. Integration runtime: default
c. Authentication type: Account key
d. Account method: From Azure Subscription Seleccionar nuestra suscription y adlssynapseey02
e. Clic en test connection
f. Clic en “Create”

[![28-create-lk.png](https://i.postimg.cc/qqjvr5s8/28-create-lk.png)](https://postimg.cc/300TC9bR)

# Linked Services: Azure SQL Database

1.- En la sección “New linked service” buscar y dar clic en ‘Azure SQL
Database’, finalmente clic en “continue”

[![29-create-lk-SQL.png](https://i.postimg.cc/VLXd5KYP/29-create-lk-SQL.png)](https://postimg.cc/ZCKTMFBw)

2.- En la ventana emergente configurar lo siguiente:

g. Name: lkdsSQL
h. Integration runtime: default
i. Authentication type: Account key
j. Account method: From Azure Subscription
Seleccionar nuestra subscription, server, database
k. Seleccionar tipo de autenticación y escribir nuestra credencial
l. Clic en test connection
m. Clic “Create”

[![30-create-lk-SQL-options.png](https://i.postimg.cc/jdh7dRQr/30-create-lk-SQL-options.png)](https://postimg.cc/LnnXxKby)

# Linked Services: Key Vault

3.- En la sección “New linked service” buscar y dar clic en ‘Key Vault’,
finalmente clic en “continue”

[![31-lks-key-vault-create.png](https://i.postimg.cc/xC7Wmrvf/31-lks-key-vault-create.png)](https://postimg.cc/9Rd83ngK)

4.- En la ventana emergente configurar lo siguiente:

n. Name: lkdsKV
o. Authentication type: Account key
p. Azure KV method: From Azure Subscription
Seleccionar nuestra subscription, key vault name
q. Authentication method: System Assigned Managed Identity
r. Clic en test connection
s. Clic “Create”

[![32-lks-key-vault-create.png](https://i.postimg.cc/sD3JJ6Ts/32-lks-key-vault-create.png)](https://postimg.cc/qgZnJLf5)

5.- Clic en “Publish all”

[![33-lks-publicsh-all.png](https://i.postimg.cc/mk7B2Zvf/33-lks-publicsh-all.png)](https://postimg.cc/646DMK6Y)

# CREACIÓN AZURE DATABRICKS

1.- Utilizar el grupo de recurso creado previamente

a. Nombre grupo de recurso: rg-datapath-synapse-002

2.-  Ingresar al grupo de recursos creado, y en la parte superior
seleccionar “Create”

[![2-ingresar-RG.png](https://i.postimg.cc/L4T7gH6y/2-ingresar-RG.png)](https://postimg.cc/NLKbWvXT)

3.- En el Marketplace escribir “azure databricks” y seleccionar el
recurso, luego clic en “create”

[![34-azure-data-bricks.png](https://i.postimg.cc/VvXMbLDB/34-azure-data-bricks.png)](https://postimg.cc/T5YhzG8p)

4.- Configurar el workspace en “basics”:

a. Resource group: rg-datapath-synapse-002
b. Workspace name: dbw-datapath-etl
c. Region: East US 2
d. Pricing Tier: Trial (Premium – 14 Days Free DBUs)
*manage resource group

[![35-create-azure-data-bricks.png](https://i.postimg.cc/wTvgvbM3/35-create-azure-data-bricks.png)](https://postimg.cc/6TDDbcdN)

Clic en “Review + create” —> “Create” —> “Go to Resource”

4.- Una vez creado el workspace, clic en “Launch Workspace”

[![36-clic-launch-workspace.png](https://i.postimg.cc/C1XXxJQn/36-clic-launch-workspace.png)](https://postimg.cc/62VYHVwB)

5.- Finalmente se abrirá una nueva ventana en el navegador con el
espacio de trabajo de Databricks

[![37-workspace-databricks.png](https://i.postimg.cc/FRCmgjnh/37-workspace-databricks.png)](https://postimg.cc/9R9vcqhk)

# CREACIÓN CLUSTER AZURE DATABRICKS

1.- En el espacio de trabajo de Databricks ir a la parte izquierda y
seleccionar “Compute”

[![37-workspace-databricks.png](https://i.postimg.cc/FRCmgjnh/37-workspace-databricks.png)](https://postimg.cc/9R9vcqhk)

2.- En la parte derecha del panel dar clic en “Create Compute”

[![38-create-compute.png](https://i.postimg.cc/L8n3Df4N/38-create-compute.png)](https://postimg.cc/4YRcNY99)

3.- Configurar el computo de nuestro cluster

a. Nombre Cluster: Datapath_Cluster
b. Policy: Unrestricted
c. Single node
d. Access Mode: No isolation shared
e. Databricks runtime version: Runtime: 10.4 LTS (Scala 2.12,
Spark 3.2.1)
f. NO seleccionar “use photon Acceleration”
g. Node Type: Por defecto
h. Terminate after “30” minutes

[![39-configuracion-cluster.png](https://i.postimg.cc/j55xns4w/39-configuracion-cluster.png)](https://postimg.cc/zLsZ9YM8)

Clic en “Create Compute”
Tiempo estimado: 3-5 minutos

4.- Explorar nuestro workspace, para crear Folder, notebooks etc.

[![40-create-folder.png](https://i.postimg.cc/Y0GvJPMh/40-create-folder.png)](https://postimg.cc/GTR3DqBC)

5.- Cargar el archivo “Datapath_07.dbc” en el workspace “shared, este archivo se encuentra en la carpeta Notebook de este repositorio

[![41-folder-Datapath-dbc.png](https://i.postimg.cc/PJvGGgJp/41-folder-Datapath-dbc.png)](https://postimg.cc/8fGXMnhN)

6.- Abrir el notebook “0 MountStorage” y seleccionar nuestro cluster

# DEFINICIÓN DELTA LAKE

1.- Ir a nuestro recurso data lake, y crear un container llamado“deltalake”

[![42-crear-container.png](https://i.postimg.cc/7L4T5Dy4/42-crear-container.png)](https://postimg.cc/HrSLNqxv)

2.- Ingresar al container “deltalake” y crear los siguientes directorios:
a. landing
b. bronze
c. silver
d. gold

[![43-crear-directorios.png](https://i.postimg.cc/RC2Y2bBf/43-crear-directorios.png)](https://postimg.cc/fkj59CkT)

3.- Ingresar a la capa “landing” y subir los archivos de la carpeta “data", estos archivos se encuentran en la carpeta  data de este repositorio.

[![44-capa-landing.png](https://i.postimg.cc/zBbsgmNz/44-capa-landing.png)](https://postimg.cc/R6mskDqy)

# CREATE A SECRET SCOPES CONNECTED TO AZURE KEY VAULT

1.- Ir al workspace, luego a nuestro notebook de “Mount Storage”,
finalmente modificar la url del workspace desde el “#”

Antes:
[![45-url-Mount-Storage.png](https://i.postimg.cc/JhDyGnsG/45-url-Mount-Storage.png)](https://postimg.cc/xkQjZ9y2)

reemplazar en parte de la url: #secrets/createScope

Después:

[![45-url-Mount-Storage-DESPUES.png](https://i.postimg.cc/tC44sTfp/45-url-Mount-Storage-DESPUES.png)](https://postimg.cc/LhGp7H2b)

Sintaxis:

https://<your_azure_databricks_url>#secrets/createScope

[![46-create-scope.png](https://i.postimg.cc/pdZS523M/46-create-scope.png)](https://postimg.cc/LqJDrMHx)

2.- Una vez realizado el paso anterior, nuestra ventana cambiará al Homepage “Create Secret Scope”

a. Scope name: “sc-adls”
b. Manage principal: “All Users”
c. CONFIGURACIÓN AZURE KA VAULT (Ver el paso 3)

3.- Ir a nuestro recurso “Key Vault”
a. Ir a properties
b. copiar y pegar DNS = Vault URI
c. Copiar y pegar Resource ID

[![47-key-vault.png](https://i.postimg.cc/Px6rynpB/47-key-vault.png)](https://postimg.cc/1nqQRTKH)

4.- Ir a nuestra venta de Secret Scope y pegar los valores previamente
copiados, finalmente clic en “Create”

[![48-create-scope-options.png](https://i.postimg.cc/yd60WBC5/48-create-scope-options.png)](https://postimg.cc/67S81k3L)

5. Aplicamos cambios, clic en “ok”

[![49-clic-ok.png](https://i.postimg.cc/3rVtRLnB/49-clic-ok.png)](https://postimg.cc/F1byCxtd)

# CREACIÓN DE SECRETS USANDO KEY VAULT

1.- Ir a nuestro recurso “Key Vault”, a la sección de Objetcs y clic en
“Secrets”, finalmente clic en “Generate/Import”

[![50-1-Acces-configuration-key-vaul.png](https://i.postimg.cc/pd1p3Lf2/50-1-Acces-configuration-key-vaul.png)](https://postimg.cc/B8HSKs7y)

[![50-2-Acces-control-IAM.png](https://i.postimg.cc/Z5DC9qC1/50-2-Acces-control-IAM.png)](https://postimg.cc/mPCbJB3w)

[![50-3-Add-role-assignment.png](https://i.postimg.cc/CKV9sGBD/50-3-Add-role-assignment.png)](https://postimg.cc/QF06XWqx)

[![50-4-Add-role-contributor.png](https://i.postimg.cc/xCjftDJK/50-4-Add-role-contributor.png)](https://postimg.cc/DWD3wNcZ)

[![50-generete-key-vaul.png](https://i.postimg.cc/2SsY7FkF/50-generete-key-vaul.png)](https://postimg.cc/gxH1Yh6n)


2.- Crear el secret para el nombre del container

a. Name: StorageAccountName

[![52-SECRET-CONTAINER.png](https://i.postimg.cc/q7qVFS32/52-SECRET-CONTAINER.png)](https://postimg.cc/Jsf2DTKh)

3.- Crear el secret para el “Access key”

a. Name: StorageAccountAccessKey (ir a nuestr data lake y hacer clic
en “Access Key”, luego copiar y pegar el key 1)

[![50-generete-key-vaul.png](https://i.postimg.cc/2SsY7FkF/50-generete-key-vaul.png)](https://postimg.cc/gxH1Yh6n)

[![53-create-secret-account-access-Key.png](https://i.postimg.cc/1tLF8r3n/53-create-secret-account-access-Key.png)](https://postimg.cc/LY357ff2)

[![54-list-secrets.png](https://i.postimg.cc/5yV0c7t3/54-list-secrets.png)](https://postimg.cc/hz2gT0Tz)

4.- Ir al workspace e iniciar el proceso

# MONTAJE DEL STORAGEACCOUNT CON DATABRICKS

1.- Verificar que los scope = "sc-adls" en el notebook de Mountstorage
2.- Verificar que en Key - Acces configuration se encuentre en la opción Vault acces policy.

[![60-montaje-storaje-account.png](https://i.postimg.cc/dV9wm9Ry/60-montaje-storaje-account.png)](https://postimg.cc/gxrCG8Tz)

# NOTEBOOK Extrac

1.- Este archivo define la estructura de cada uno de los archivos que se encuentran en nuestro deltalake en la capa landing.

2.- Guarda la información a la capa bronze en formato delta, esto con la finalidad de mejorar los tiempos de rendimiento, performance y harán que las consultas sean mucho más rápido.

[![61-definicion-estructura.png](https://i.postimg.cc/pTzBYVcd/61-definicion-estructura.png)](https://postimg.cc/wyxJ9zpK)

# NOTEBOOK Transform

1.- Este archivo tiene las reglas de negocio, transformaciones y realizada el volcado de información sobre la capa silver

[![62-Trnasform-notebook.png](https://i.postimg.cc/SxNRVdd9/62-Trnasform-notebook.png)](https://postimg.cc/Lnwm8zr4)

# NOTEBOOK Load

1.-  Este archivo realiza la carga de datos a la capa gold, para nuestro caso solo esta leyendo y volviendo a cargar sobre la capa gold pero si quisieramos llevarlo a una base de datos SQL Database, CosmosDb este sería quien debe contener la lógica de carga.

[![63-Load-notebook.png](https://i.postimg.cc/tCtJbwJw/63-Load-notebook.png)](https://postimg.cc/7fhwMBgn)

# NOTEBOOK Main

1.-  Este archivo llama la Extracción, Transformación y carga y lo estoy llamando en un solo Notebook, la funcionalidad que permite realizar esto es:

		%run "../Datapath_M07/ETL/Extract"

# CREACIÓN LINKED SERVICES DATABRICKS

1.- Ir al grupo al ADF Studio, dar clic en “Manage” en la sección de
connections clic en “Linked services” finalmente clic en “New”

2. En la sección “New linked service”, seleccionar “Compute” luego buscar y dar clic en ‘Azure Databricks’, finalmente clic en “continue”

[![55-create-lk-Azure-Databricks.png](https://i.postimg.cc/Gm2vS2Vp/55-create-lk-Azure-Databricks.png)](https://postimg.cc/D89WS22V)

3.- En la ventana emergente configurar lo siguiente:

a. Name: lkdsDatabricks
b. Integration runtime: default
c. Account selection method: From Azure Suscription
Seleccionar nuestra suscription y databricks workspace
d. Select cluster: “Existing interactive cluster”
e. Authentication type: “Access Token” (para ver como obtener, revisar
los siguientes pasos)

4.- Ir al workspace de Databricks y seleccionar “User settings”
5.- Ir la sección de “Developer”, luego a “access token”, clic en “Manage”

[![56-user-settings.png](https://i.postimg.cc/LsfX5KGS/56-user-settings.png)](https://postimg.cc/yJ1Bp5Vp)

6.- Clic en “Generate new token”
7.- Escribir un comentario “Datapath” luego clic en “Generate”

[![57-generate-token.png](https://i.postimg.cc/FsmnnyhV/57-generate-token.png)](https://postimg.cc/6Tj08G28)

8. Copiar el token generado en un bloc de notas, luego clic en “Done”

[![58-generate-token-new.png](https://i.postimg.cc/sXrpDJ6n/58-generate-token-new.png)](https://postimg.cc/jWMWMyrN)

9.- Regresar a la creación de linked services y pegar en “Access token” el token generado anteriormente.

10.- En la sección “Choose from existing clusters” seleccionamos nuestro cluster

11. Clic en “test connection”, luego clic en “create

[![59-create-lk-databricks.png](https://i.postimg.cc/7LmxfHXp/59-create-lk-databricks.png)](https://postimg.cc/gwL97PCD)

# CREACIÓN & SCHEDULING DE PIPELINE USANDO ADF

1.- Ir al grupo al ADF Studio, dar clic en “Author” en la sección de Pipelines
clic en los tres puntos, finalmente clic en “New pipeline”

[![64-new-pipeline.png](https://i.postimg.cc/JzHNwhxr/64-new-pipeline.png)](https://postimg.cc/Xr3Gd4tm)

En properties, colocar nombre: pipeline_0001_etl_databricks

2.- En la sección de “Activities”, ir a la parte de “Databricks” y arrastrar
“Notebook” al espacio de trabajo.

[![65-activities-databricks.png](https://i.postimg.cc/PrY6Z2Gz/65-activities-databricks.png)](https://postimg.cc/XpNc6KYp)

3.- Clic sobre el notebook y realizar las siguientes configuraciones:
a. General:
i. Name: Extract
b. Azure Databricks: seleccionar nuestro linked services
c. Settings:
i. Notebook path: clic en “Browse” y seleccionar la ruta de
nuestro notebook, hasta llegar al notebook “Extract”

[![66-configuration-activitys.png](https://i.postimg.cc/3NNbGv5M/66-configuration-activitys.png)](https://postimg.cc/hQFrWhtM)

4.- Arrastrar al espacio de trabajo dos notebooks y realizar el paso 3 para el
notebook de “Transform” y “Load”

[![67-union-activitys.png](https://i.postimg.cc/nLkXjsGG/67-union-activitys.png)](https://postimg.cc/Y4vrTCVv)

5.- Unir los notebook teniendo en cuenta la característica “On success”, clic en“Publish all”

[![68-debugs.png](https://i.postimg.cc/yYrRTpt3/68-debugs.png)](https://postimg.cc/4mVnfPJX)

[![71-pipeline-MOUNT-STORAGE.png](https://i.postimg.cc/ZYCQTn3G/71-pipeline-MOUNT-STORAGE.png)](https://postimg.cc/XX6LLVsL)

# CREACIÓN PIPELINE PADRE

1.- en la sección de Pipelines clic en los tres puntos, finalmente clic en “New pipeline”

2.- En properties, colocar nombre: pipeline_0001_Master

3.- En la sección de “Activities”, ir a la parte de “General” y arrastrar
“Execute Pipeline1” al espacio de trabajo

[![69-pipeline-master.png](https://i.postimg.cc/brrxz748/69-pipeline-master.png)](https://postimg.cc/rR60jP8Y)

4.- Clic sobre el Execute pipeline y realizar las siguientes configuraciones:
a. General:
b. Name: mount_storage
c. Settings:
d. Invoked pipeline: clic en “select” y seleccionar pipeline_0001_mount_storage

5.- El mismo procedimiento realizamos para invocar al pipeline_001_etl_databricks

6.- En la sección de “Activities”, ir a la parte de “Databricks” y arrastrar
“Notebook” al espacio de trabajo y configuramos para que ejcute el notebook Main.


[![72-PIPELINE-MAIN.png](https://i.postimg.cc/9Qbn17Zn/72-PIPELINE-MAIN.png)](https://postimg.cc/JsyxztTN)

7.- Clic en Trigger para Schedular el pipeline

[![73-create-trigger.png](https://i.postimg.cc/wT2Mz9Ns/73-create-trigger.png)](https://postimg.cc/BX84c9Mq)

8. Clic en “publish all” para guardar cambios.
