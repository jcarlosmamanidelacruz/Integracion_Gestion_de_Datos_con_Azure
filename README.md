# Solución de Integración y Gestión de Datos en la Nube con Azure

[![Azure-Integration.png](https://i.postimg.cc/9QtVhCBG/Azure-Integration.png)](https://postimg.cc/tsT8d0wT)

## Descripción del Proyecto

En este proyecto, se ha desplegado una solución completa de análisis de datos utilizando Microsoft Azure. El proceso abarcó la creación y configuración de varios servicios de Azure, incluyendo Azure Synapse Analytics, Azure Data Factory, Azure SQL Database, Azure Key Vault, Azure Databricks, una máquina virtual para simular un entorno local con Integration Runtime, y la integración con Power BI para la visualización de datos. 

A continuación, se presentan las conclusiones clave de cada componente y las mejores prácticas observadas:

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

## Máquina Virtual con Integration Runtime

- Simulación de Entorno Local: Se instaló Integration Runtime en una máquina virtual para simular un entorno local y facilitar la transferencia de datos a Azure Data Lake.

- Conectividad Segura: Permite la transferencia de datos desde el entorno local a la nube de manera segura y eficiente.

## Power BI

- Visualización de Datos: Integra Power BI para la creación de dashboards y reportes que permiten responder preguntas de negocio de la alta gerencia.

- Interactividad y Análisis: Facilita la exploración interactiva y el análisis de datos para obtener insights accionables.

## Integración y Orquestación

- Solución End-to-End: La integración de todos estos servicios permite construir una solución de análisis de datos end-to-end que es escalable, segura y eficiente.

- Gestión de Permisos: La configuración adecuada de los permisos y la gestión de identidades es fundamental para asegurar que cada componente pueda interactuar correctamente y de manera segura.

## Replicación del Proyecto

Este proyecto está diseñado paso a paso para que cualquier persona pueda replicarlo siguiendo la documentación detallada disponible en este repositorio. Cada paso, desde la creación de los servicios de Azure hasta la configuración y ejecución de los flujos de datos, se documenta exhaustivamente para facilitar la reproducción del proyecto en cualquier entorno Azure.

------------

# CREACIÓN SYNAPSE ANALYTICS

1 . Iniciar Sesión en Azure Portal y crear grupo de recursos

- **Nombre grupo de recursos:** rg-datapath-synapse-002

- **Region**: (US) East US 2

[![crear-grupo-recursos.png](https://i.postimg.cc/cC4xSXX0/crear-grupo-recursos.png)](https://postimg.cc/YGZKNN5s)

2 . Ingresar al grupo de recursos creado, y en la parte superior seleccionar “Create"

[![2-ingresar-RG.png](https://i.postimg.cc/L4T7gH6y/2-ingresar-RG.png)](https://postimg.cc/NLKbWvXT)

3 . En el Marketplace escribir “azure synapse analytics” y seleccionar el recurso, luego clic en “create”

[![3-create-Synaps-Analytics.png](https://i.postimg.cc/d0sZ1mWN/3-create-Synaps-Analytics.png)](https://postimg.cc/YvPCDgBQ)

4 . Configurar el workspace en “basics”:

- **a. Manage resource group: **rg-manage-synapse-jc

- **b. Workspace name:** synw-serverless-jc (nombre debe ser único)

- **c. Region:** East US

[![4-configuracion-workspace.png](https://i.postimg.cc/PqnNcGvw/4-configuracion-workspace.png)](https://postimg.cc/3k9K4cB8)

Creamos un nuevo Azure Data lake Storage

- **d. Account name:** clic en “create new” → adlssynapseey02 (nombre debe ser único)

- **e. File system name:** clic en “create new” → source

[![5-creacion-datalake-storage.png](https://i.postimg.cc/GhBFnCt7/5-creacion-datalake-storage.png)](https://postimg.cc/N5c9RZ1m)

5 . Configurar el workspace en “Security”:

- **a. Authentication method:** Use both local and Azure Active Directory (Azure AD) authentication

- **b. SQL Server admin login:** sqladminuser

- **c.** Escribir y confirmar contraseña
- 	
[![6-configuracion-security.png](https://i.postimg.cc/R06MRgZD/6-configuracion-security.png)](https://postimg.cc/XZWSNcFf)

6 . Clic en “review + create” → Create

Nota: Al momento de crearSsynapse, por defecto nos va a crear el Synapse serverless pool.

# CREACIÓN AZURE DATA FACTORYCREACIÓN AZURE DATA FACTORY

1.- Ir al grupo de recursos creado **"rg-datapath-synapse-002"**, luego ir a **“+ Create”**, finalmente en el Marketplace escribir **“azure data factory”** y seleccionar “**Create”**

[![7-crear-azure-DF.png](https://i.postimg.cc/9MNgpHJ6/7-crear-azure-DF.png)](https://postimg.cc/HcXtYRGz)

2.- En la sección “**Basics”** configurar lo siguiente:

- **a. Resource group:** rg-datapath-synapse-002

- **b. Name:** adf-emissions-jc

- **c. Region:** East US 2

- **d. Version:** V2

[![8-configuracion-DF.png](https://i.postimg.cc/xCJjgn9H/8-configuracion-DF.png)](https://postimg.cc/xJQS1rJT)

Clic en “Review + Create” —> “Create” —> “Go to resource”

3.- Dentro del recurso creado clic en “Launch studio”

[![9-DF-creado.png](https://i.postimg.cc/K8DdXV0W/9-DF-creado.png)](https://postimg.cc/jDC8P806)

# CREACIÓN AZURE SQL DATABASE

1.- Ir al grupo de recursos creado **"rg-datapath-synapse-002"**, luego ir a **“+ Create”**, finalmente en el Marketplace escribir **“sql database”** y seleccionar **“Create”**

[![10-create-SQL-database.png](https://i.postimg.cc/vTpcsDZG/10-create-SQL-database.png)](https://postimg.cc/m1yb3L9n)

2.- En la sección **“Basics”** configurar lo siguiente:

- **a. Resource group:** rg-datapath-synapse-001

- **b. Database Name:** db-datapath

- **c. Server:** “Create New”

[![11-create-SQL-database-configuracion.png](https://i.postimg.cc/9XgsPKMP/11-create-SQL-database-configuracion.png)](https://postimg.cc/xqHsnsLq)

C.1 **“Create New”**

- **a. Server name:** server-datapath

- **b. Location:** (US) East US 2

- **c. Authentication method:** Use SQL authentication

- **d. Server admin login:** username

[![12-create-SQL-database-server.png](https://i.postimg.cc/9fNNPqXm/12-create-SQL-database-server.png)](https://postimg.cc/kVKcqGtz)

Clic en **“OK”**

- **d. Want to use SQL elastic pool?:** No

- **e. Workload environment:** Development

- **f. Backup storage:** Local


[![13-Sql-elasic-pool.png](https://i.postimg.cc/Qx44ZT5p/13-Sql-elasic-pool.png)](https://postimg.cc/vcnLWD0B)

Clic en **“Review + Create” —> “Create” —> “Go to resource”**

3.- Ir a **"query editor (preview)”**

Ingresar con las credenciales, nos aparecerá un error debido al firewall

[![14-Sql-elasic-pool.png](https://i.postimg.cc/MphbDb2p/14-Sql-elasic-pool.png)](https://postimg.cc/87m6kvz2)

4.- Ir a **“Overview”** luego en la parte superior central ir a **“ Set server firewall” →“Public access” —> “select networks”**

[![15-selected-networks.png](https://i.postimg.cc/yNPZgwKc/15-selected-networks.png)](https://postimg.cc/B8j6R7Xn)

5.- Ir a la sección de **“Firewall rules”** luego clic en **“ Add a firewall rule”** a
continuación colocar nuestra ip de conexión a internet, finalmente **“save”**

[![16-add-ip-rules.png](https://i.postimg.cc/Qd9pnX7b/16-add-ip-rules.png)](https://postimg.cc/Hrmcr1Bc)

6.- Ir a **“Query editor”** ingresar con nuestras credenciales

7.- Abrir un **“New Query”** y pegar la siguiente consulta:

		CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL,
			FirstName varchar(50),
			LastName varchar(50)
		)
		GO
		
		
7-. Ir a nuestro y dentro de nuestro contenedor **“source”** crear una carpeta llamada **"input"** finalmente subir archivo “emp.txt”

# CREACIÓN AZURE KEY VAULT

1.- Ir al grupo de recursos creado **"rg-datapath-synapse-002"**, luego ir a **“+ Create”**, finalmente en el Marketplace escribir **“key vault”** y seleccionar **“Create"**

[![17-create-key-vault.png](https://i.postimg.cc/nzHpmqqh/17-create-key-vault.png)](https://postimg.cc/McrC2MsC)

2.- En la sección **“Basics”** configurar lo siguiente:

- **a. Resource group:** rg-datapath-synapse-002

- **b. Key vault Name:** kv-datapath-jc

- **c. Region: **East US 2

- **d. Pricing Tier:** Standard

[![18-create-key-vault-optons.png](https://i.postimg.cc/xdVbWFVp/18-create-key-vault-optons.png)](https://postimg.cc/VJ46tg6X)

Clic en **“Review + Create” —> “Create” —> “Go to resource”**

# OTORGAR PERMISOS

1.- Ir a “Access control (IAM), luego **“Add”**, clic en **“Add role assignment”**

[![19-add-roles.png](https://i.postimg.cc/Vs04Qpxm/19-add-roles.png)](https://postimg.cc/XX399HNz)

2.- En **“Job function roles”** seleccionar **“key vault contributor”**, luego clic en**“Members”**

[![20-add-roles-member.png](https://i.postimg.cc/W33Wk4Yk/20-add-roles-member.png)](https://postimg.cc/Y45zctHr)

3.- En **“Assign access to”** seleccionar **‘User, group, or service principal’**, luego en **“Members”** clic en **‘+ Select members’**

[![21-add-roles-member-select.png](https://i.postimg.cc/zGDpHBcF/21-add-roles-member-select.png)](https://postimg.cc/rKb5v8cD)

4.- En la ventana abierta escribir nuestro usuario y selecionarlo

[![22-select-member.png](https://i.postimg.cc/qqVHpnQL/22-select-member.png)](https://postimg.cc/vgzNz4Jg)

Clic en **“select” —> “Review + assign”**

5.- Repetimos los pasos para asignar permisos a nuestro servicio Azure Data
Factory, cambiando en el paso 3 **“Assign access”** a **‘Manage identity’**

[![23-select-member-managed-identity.png](https://i.postimg.cc/mgJRgtLt/23-select-member-managed-identity.png)](https://postimg.cc/sMYbnfZr)

6.- Seleccionar nuestro servicio, finalmente clic en **“select”**

[![24-select-member-managed-identity-assign.png](https://i.postimg.cc/BvPFjL2y/24-select-member-managed-identity-assign.png)](https://postimg.cc/TyG1SP7q)

Doble clic en **“Review + assign”**

7.- Los permisos deberán aparecer así:

[![25-permisos-finales.png](https://i.postimg.cc/TYXvSZkW/25-permisos-finales.png)](https://postimg.cc/MXtLfrJW)

# CREACIÓN LINKED SERVICES ADF

1.- Ir al grupo al ADF Studio, dar clic en **“Manage”** en la sección de connections clic en **“Linked services”** finalmente clic en **“New”**

[![26-linked-services.png](https://i.postimg.cc/3JLJkVP6/26-linked-services.png)](https://postimg.cc/WhkPKWp6)

2.- En la sección **“New linked service”** buscar y dar clic en **‘Azure Data Lake Storage Gen 2’**, finalmente clic en **“continue”**

[![27-azure-data-lake.png](https://i.postimg.cc/6QqH92Cg/27-azure-data-lake.png)](https://postimg.cc/4KCvP3LQ)

3.- En la ventana emergente configurar lo siguiente:

- **a. Name:** lkdsADLS

- **b. Integration runtime:** default

- **c. Authentication type:** Account key

- **d. Account method:** From Azure Subscription Seleccionar nuestra suscription y adlssynapseey02

- **e.** Clic en **test connection**

- **f.** Clic en **“Create”**

[![28-create-lk.png](https://i.postimg.cc/qqjvr5s8/28-create-lk.png)](https://postimg.cc/300TC9bR)

# Linked Services: Azure SQL Database

1.- En la sección **“New linked service”** buscar y dar clic en **‘Azure SQL Database’**, finalmente clic en **“continue”**

[![29-create-lk-SQL.png](https://i.postimg.cc/VLXd5KYP/29-create-lk-SQL.png)](https://postimg.cc/ZCKTMFBw)

2.- En la ventana emergente configurar lo siguiente:

- **g. Name:** lkdsSQL

- **h. Integration runtime: ** default

- **i. Authentication type:** Account key

- **j. Account method:** From Azure Subscription, Seleccionar nuestra subscription, server, database

- **k.** Seleccionar tipo de autenticación y escribir nuestra credencial

- **l. Clic en **test connection**

- **m.** Clic **“Create”**

[![30-create-lk-SQL-options.png](https://i.postimg.cc/jdh7dRQr/30-create-lk-SQL-options.png)](https://postimg.cc/LnnXxKby)

# Linked Services: Key Vault

3.- En la sección **“New linked service”** buscar y dar clic en **‘Key Vault’**, finalmente clic en **“continue”**

[![31-lks-key-vault-create.png](https://i.postimg.cc/xC7Wmrvf/31-lks-key-vault-create.png)](https://postimg.cc/9Rd83ngK)

4.- En la ventana emergente configurar lo siguiente:

- **Name:** lkdsKV

- **Authentication type: **Account key

- **Azure KV method:** From Azure Subscription, Seleccionar nuestra subscription, key vault name
- **Authentication method:** System Assigned Managed Identity

-  Clic en **test connection**

-  Clic **“Create”**

[![32-lks-key-vault-create.png](https://i.postimg.cc/sD3JJ6Ts/32-lks-key-vault-create.png)](https://postimg.cc/qgZnJLf5)

5.- Clic en **“Publish all”**

[![33-lks-publicsh-all.png](https://i.postimg.cc/mk7B2Zvf/33-lks-publicsh-all.png)](https://postimg.cc/646DMK6Y)

## CREACION DE CONTAINER

Nos dirigimos a nuestro recurso **adlssynapseey02** Data Lake Storage Gen2, damos Clic en **Containers** 

- Creamos un container **project**
- Dentro del container  **project** agregamos los siguientes directorios: **landing,  bronze, silver, gold.**

[![115-datalake.png](https://i.postimg.cc/cC7J1p7t/115-datalake.png)](https://postimg.cc/rRmTQYPq)


[![114-create-container.png](https://i.postimg.cc/44Yb4BhL/114-create-container.png)](https://postimg.cc/jCrJZ6cP)

# CREACIÓN AZURE DATABRICKS

1.- Utilizar el grupo de recurso creado previamente

- **Nombre grupo de recurso: **rg-datapath-synapse-002

2.-  Ingresar al grupo de recursos creado, y en la parte superior seleccionar **“Create”**

[![2-ingresar-RG.png](https://i.postimg.cc/L4T7gH6y/2-ingresar-RG.png)](https://postimg.cc/NLKbWvXT)

3.- En el Marketplace escribir **“azure databricks”** y seleccionar el recurso, luego clic en **“create”**

[![34-azure-data-bricks.png](https://i.postimg.cc/VvXMbLDB/34-azure-data-bricks.png)](https://postimg.cc/T5YhzG8p)

4.- Configurar el workspace en **“basics”:**

- **a. Resource group:** rg-datapath-synapse-002

- **b. Workspace name:** dbw-datapath-etl

- **c. Region:** East US 2

- **d. Pricing Tier:** Trial (Premium – 14 Days Free DBUs)


[![35-create-azure-data-bricks.png](https://i.postimg.cc/wTvgvbM3/35-create-azure-data-bricks.png)](https://postimg.cc/6TDDbcdN)

Clic en **“Review + create” —> “Create” —> “Go to Resource”**

4.- Una vez creado el workspace, clic en **“Launch Workspace”**

[![36-clic-launch-workspace.png](https://i.postimg.cc/C1XXxJQn/36-clic-launch-workspace.png)](https://postimg.cc/62VYHVwB)

5.- Finalmente se abrirá una nueva ventana en el navegador con el
espacio de trabajo de Databricks

[![37-workspace-databricks.png](https://i.postimg.cc/FRCmgjnh/37-workspace-databricks.png)](https://postimg.cc/9R9vcqhk)

# CREACIÓN CLUSTER AZURE DATABRICKS

1.- En el espacio de trabajo de Databricks ir a la parte izquierda y
seleccionar **“Compute”**

[![37-workspace-databricks.png](https://i.postimg.cc/FRCmgjnh/37-workspace-databricks.png)](https://postimg.cc/9R9vcqhk)

2.- En la parte derecha del panel dar clic en **“Create Compute”**

[![38-create-compute.png](https://i.postimg.cc/L8n3Df4N/38-create-compute.png)](https://postimg.cc/4YRcNY99)


3.- Configurar el computo de nuestro cluster

- **Nombre Cluster:** Datapath_Cluster

- **Policy:** Unrestricted

- **Single node**

- **Access Mode:** No isolation shared

- **Databricks runtime version:** Runtime: 10.4 LTS (Scala 2.12,
- Spark 3.2.1)

- **NO seleccionar** “use photon Acceleration”

- **Node Type:** Por defecto

- **Terminate after** “30” minutes

[![39-configuracion-cluster.png](https://i.postimg.cc/j55xns4w/39-configuracion-cluster.png)](https://postimg.cc/zLsZ9YM8)

Clic en **“Create Compute”** 

Tiempo estimado: 3-5 minutos

4.- Explorar nuestro **workspace**, para crear Folder, notebooks etc.

[![40-create-folder.png](https://i.postimg.cc/Y0GvJPMh/40-create-folder.png)](https://postimg.cc/GTR3DqBC)

5.- Cargar el archivo **“retail.dbc”** en el workspace **“shared"**, este archivo se encuentra en la carpeta **Notebook ** de este repositorio

[![41-folder-Datapath-dbc.png](https://i.postimg.cc/PJvGGgJp/41-folder-Datapath-dbc.png)](https://postimg.cc/8fGXMnhN)

6.- Abrir el notebook “0 MountStorage” y seleccionar nuestro cluster

# CREATE A SECRET SCOPES CONNECTED TO AZURE KEY VAULT

1.- Ir al workspace, luego a nuestro notebook de **“Mount Storage”**,
finalmente modificar la url del workspace desde el **“#”**

**Antes:**
[![45-url-Mount-Storage.png](https://i.postimg.cc/JhDyGnsG/45-url-Mount-Storage.png)](https://postimg.cc/xkQjZ9y2)

reemplazar en parte de la url: #secrets/createScope

**Después:**

[![45-url-Mount-Storage-DESPUES.png](https://i.postimg.cc/tC44sTfp/45-url-Mount-Storage-DESPUES.png)](https://postimg.cc/LhGp7H2b)

**Sintaxis:**

https://<your_azure_databricks_url>#secrets/createScope

[![46-create-scope.png](https://i.postimg.cc/pdZS523M/46-create-scope.png)](https://postimg.cc/LqJDrMHx)

2.- Una vez realizado el paso anterior, nuestra ventana cambiará al Homepage **“Create Secret Scope”**

- **Scope name:** “sc-adls”

- **Manage principal:** “All Users”

**CONFIGURACIÓN AZURE KEY VAULT (Ver el paso 3)**

3.- Ir a nuestro recurso **“Key Vault”**

- **Ir a properties**

- **copiar y pegar DNS = Vault URI**

- **Copiar y pegar Resource ID**

[![47-key-vault.png](https://i.postimg.cc/Px6rynpB/47-key-vault.png)](https://postimg.cc/1nqQRTKH)

4.- Ir a nuestra ventana de **Secret Scope **y pegar los valores previamente
copiados, finalmente clic en **“Create”**

[![48-create-scope-options.png](https://i.postimg.cc/yd60WBC5/48-create-scope-options.png)](https://postimg.cc/67S81k3L)

5. Aplicamos cambios, clic en **“ok”**

[![49-clic-ok.png](https://i.postimg.cc/3rVtRLnB/49-clic-ok.png)](https://postimg.cc/F1byCxtd)

# CREACIÓN DE SECRETS USANDO KEY VAULT

1.- Ir a nuestro recurso **“Key Vault”**, a la sección de Objetcs y clic en **“Secrets”**, finalmente clic en **“Generate/Import”**

[![50-1-Acces-configuration-key-vaul.png](https://i.postimg.cc/pd1p3Lf2/50-1-Acces-configuration-key-vaul.png)](https://postimg.cc/B8HSKs7y)

[![50-2-Acces-control-IAM.png](https://i.postimg.cc/Z5DC9qC1/50-2-Acces-control-IAM.png)](https://postimg.cc/mPCbJB3w)

[![50-3-Add-role-assignment.png](https://i.postimg.cc/CKV9sGBD/50-3-Add-role-assignment.png)](https://postimg.cc/QF06XWqx)

[![50-4-Add-role-contributor.png](https://i.postimg.cc/xCjftDJK/50-4-Add-role-contributor.png)](https://postimg.cc/DWD3wNcZ)

[![50-generete-key-vaul.png](https://i.postimg.cc/2SsY7FkF/50-generete-key-vaul.png)](https://postimg.cc/gxH1Yh6n)

2.- Crear el secret para el nombre del container

- ** Name: ** StorageAccountName

[![52-SECRET-CONTAINER.png](https://i.postimg.cc/q7qVFS32/52-SECRET-CONTAINER.png)](https://postimg.cc/Jsf2DTKh)

3.- Crear el secret para el **“Access key”**

- **Name:**  StorageAccountAccessKey (ir a nuestr data lake y hacer clic en “Access Key”, luego copiar y pegar el key 1)

[![50-generete-key-vaul.png](https://i.postimg.cc/2SsY7FkF/50-generete-key-vaul.png)](https://postimg.cc/gxH1Yh6n)

[![53-create-secret-account-access-Key.png](https://i.postimg.cc/1tLF8r3n/53-create-secret-account-access-Key.png)](https://postimg.cc/LY357ff2)

[![54-list-secrets.png](https://i.postimg.cc/5yV0c7t3/54-list-secrets.png)](https://postimg.cc/hz2gT0Tz)

4.- Crear el secret para el **ServerName**

- **Name:**  ServerName
- **Secret vault: ** (ir a nuestro Sql Database y copiamos el Server name de nuestro recuerso Sql Database)
- Click en **Create**

5.- Crear el secret para el **DatabaseName**

- **Name:**  DatabaseName
- **Secret vault: ** (ir a nuestro Sql Database y copiamos el nombre de la base de datos)

- Click en **Create**

6.- Crear el secret para el **UserName**

- **Name:**  UserName
- **Secret vault: ** (ir a nuestro Sql Database y copiamos el nombre del usuario que configuramos al momentos de crear nuestro SQL Database)

- Click en **Create**

7.- Crear el secret para el **PasswordDatabase**

- **Name:**  PasswordDatabase
- **Secret vault: ** (colocamos el Password  que ingresamos al momento que creamos nuestro SQL Database)

[![74-list-secrets.png](https://i.postimg.cc/htrm3kpz/74-list-secrets.png)](https://postimg.cc/DSSmSMVn)

8.- Regresamos a **Acces configuration** y le damos en la opción **Vault access policy** y damos clic en Apply

[![75-Acces-configuration-vault-acces-policy.png](https://i.postimg.cc/J0xqcsD0/75-Acces-configuration-vault-acces-policy.png)](https://postimg.cc/ctKwd4cG)

7.- Ir al workspace e iniciar el proceso

# MONTAJE DEL STORAGEACCOUNT CON DATABRICKS

1.- Verificar que los scope = "sc-adls" en el notebook de Mountstorage

2.- Verificar que en Key - Acces configuration se encuentre en la opción Vault acces policy.

[![76-verificamos-mount-storage.png](https://i.postimg.cc/FHNhFZL3/76-verificamos-mount-storage.png)](https://postimg.cc/kB10fWwg)

# NOTEBOOK Extrac

1.- Este archivo define la estructura de cada uno de los archivos que se encuentran en nuestro deltalake en la capa landing.

2.- Guarda la información a la capa bronze en formato delta, esto con la finalidad de mejorar los tiempos de rendimiento, performance y harán que las consultas sean mucho más rápido.

[![77-notebook-extrac.png](https://i.postimg.cc/SsDMVRbt/77-notebook-extrac.png)](https://postimg.cc/yg3x8VdF)

# NOTEBOOK Transform

1.- Este archivo tiene las reglas de negocio, transformaciones y realizada el volcado de información sobre la capa silver

[![77-notebook-Transform.png](https://i.postimg.cc/Njq56mrj/77-notebook-Transform.png)](https://postimg.cc/0bnkx6fT)

# NOTEBOOK Load

1.-  Este archivo realiza la carga de datos a la base de datos SQL Database.

[![78-notebook-Load.png](https://i.postimg.cc/mgBVdcvT/78-notebook-Load.png)](https://postimg.cc/SJ1WRKQP)

NOTEBOOK Main

1.- Este archivo llama la Extracción, Transformación y carga y lo estoy llamando en un solo Notebook, la funcionalidad que permite realizar esto es:

	%run "../Datapath_M07/ETL/Extract"


# CREACIÓN LINKED SERVICES DATABRICKS

1.- Ir al grupo al ADF Studio, dar clic en **“Manage”** en la sección de
connections clic en **“Linked services”** finalmente clic en **“New”**

2. En la sección **“New linked service**”, seleccionar **“Compute”** luego buscar y dar clic en **‘Azure Databricks’**, finalmente clic en **“continue”**

[![55-create-lk-Azure-Databricks.png](https://i.postimg.cc/Gm2vS2Vp/55-create-lk-Azure-Databricks.png)](https://postimg.cc/D89WS22V)

3.- En la ventana emergente configurar lo siguiente:

- ** Name:** lkdsDatabricks
- ** Integration runtime:** default
- **Account selection method:** From Azure Suscription, Seleccionar nuestra suscription y databricks workspace
- **Select cluster:** “Existing interactive cluster”
- **Authentication type: **“Access Token” (para ver como obtener, revisar los siguientes pasos)

4.- Ir al workspace de Databricks y seleccionar **“User settings”**

5.- Ir la sección de** “Developer”**, luego a **“access token”**, clic en **“Manage”**

[![56-user-settings.png](https://i.postimg.cc/LsfX5KGS/56-user-settings.png)](https://postimg.cc/yJ1Bp5Vp)

6.- Clic en **“Generate new token”**

7.- Escribir un comentario **“Datapath”** luego clic en **“Generate”**

[![57-generate-token.png](https://i.postimg.cc/FsmnnyhV/57-generate-token.png)](https://postimg.cc/6Tj08G28)

8. Copiar el token generado en un bloc de notas, luego clic en **“Done”**

[![58-generate-token-new.png](https://i.postimg.cc/sXrpDJ6n/58-generate-token-new.png)](https://postimg.cc/jWMWMyrN)

9.- Regresar a la creación de linked services y pegar en **“Access token”** el token generado anteriormente.

10.- En la sección **“Choose from existing clusters”** seleccionamos nuestro cluster

11. Clic en **“test connection”**, luego clic en ** create**

[![59-create-lk-databricks.png](https://i.postimg.cc/7LmxfHXp/59-create-lk-databricks.png)](https://postimg.cc/gwL97PCD)

# CREACIÓN DE VIRTUAL MACHINE

1 . Ingresar al grupo de recursos creado, y en la parte superior seleccionar “Create"

2 . En el Marketplace escribir “virtual machine” y seleccionar el recurso, luego clic en “create”

a. Manage resource group: rg-manage-synapse-jc b. Workspace name: synw-serverless-jc (nombre debe ser único) c. Region: East US

- **Virtual machine name:** vm-retail-jcmd-001
- **Region:** (US) East US 2
- **Availability options:** No infrastructure redundancy required
- **Security type:** Standard
- **Image:** Windows Server 2019 Datacenter - x64 Gen2
- **VM architecture:** x64

[![81-create-virtual-machine.png](https://i.postimg.cc/m2bxrcFJ/81-create-virtual-machine.png)](https://postimg.cc/Sjt1Txdf)

- **Run with Azure Spot discount:** sin seleccionar
- **Size:** Standard_DS1_v2 - 1 vcpu, 3.5 GiB memory (85,41 US$/month)
- **Enable Hibernation:** sin seleccionar
- **Username:** vm-retail-prod
- **Password:** colocar el password
- **Confirm Password:**colocar el password
- **Public inbound ports:** Allow selected ports
- **Select inbound ports:** RDP (3389)

Luego Clic en **Disks**

[![82-create-virtual-machine-2.png](https://i.postimg.cc/HshFp60v/82-create-virtual-machine-2.png)](https://postimg.cc/rK5fjCP5)

- **Encryption at host: ** sin seleccionar el check
- **OS disk size: ** Image default (127 GiB)
- **OS disk type: ** Standard HDD (locally-redundant storage)
- **Delete with VM: ** darle check
- **Key management: ** Platform-managed key
- **Enable Ultra Disk compatibility: **  sin seleccionar el check

Luego Clic en **Networking**

[![83-create-virtual-machine-disks.png](https://i.postimg.cc/76LYFBnP/83-create-virtual-machine-disks.png)](https://postimg.cc/06Lsm04h)

- ** Virtual network: ** la que viene por default
- ** Subnet: ** la que viene por default
- ** Public IP: ** la que viene por default
- ** NIC network security group : **Basic
- ** Public inbound ports: ** Allow selected ports
- ** Select inbound ports: ** RDP (3389)
- ** Delete NIC when VM is deleted: ** sin seleccionar el check
- ** Enable accelerated networking: ** darle check
- ** Load balancing options: ** None

Luego Clic en **Management**


[![83-create-virtual-machine-network.png](https://i.postimg.cc/9X7hN46q/83-create-virtual-machine-network.png)](https://postimg.cc/CdSXRKLF)

En esta opción configurar las opciones de acuerdo a la imagen, cambiando la **zona horaria** de acuerdo a tu país y el **Email** por el de tu cuenta en AzurePortal.

[![84-create-virtual-machine-management.png](https://i.postimg.cc/769bbKkd/84-create-virtual-machine-management.png)](https://postimg.cc/S2Xmv7jf)

En las opciones de **Advanced **, ** Tags** dejar las que vienen por default y dar clic en **Review + create**

[![85-create-virtual-machine-reviwe-create.png](https://i.postimg.cc/W4DQdzPv/85-create-virtual-machine-reviwe-create.png)](https://postimg.cc/8FSy315K)

[![85-create-virtual-machine-reviwe-deployment.png](https://i.postimg.cc/W4QxgV2J/85-create-virtual-machine-reviwe-deployment.png)](https://postimg.cc/060t1LK8)


## Conectarnos a la VM

En nuestro buscador de Windows buscamos Conexión a Escritorio remoto y damos Clic.

- **Escriba el nombre del equipo remoto: ** nos dirigimos a nuestra maquina virtual de nuestros recursos y copiamos el **Public IP address**

- **Usuario: ** colocamos el **UserName** que ingresamos al momento de configurar nuesto VM para nuestro caso **vm-retail-prod**

- **Escribir las credenciales: ** colocamos el **Password** que ingresamos al momento de configurar nuesto VM.

Damos Clic en ** Aceptar**

[![86-conexion-remota.png](https://i.postimg.cc/cCBnffnt/86-conexion-remota.png)](https://postimg.cc/Fk1z4dGh)

En la siguiente ventana le damos Clic a **Yes**

[![87-vm-DENTRO.png](https://i.postimg.cc/WpMxqthG/87-vm-DENTRO.png)](https://postimg.cc/0ryVLkGj)

Ingresamos al navegador de **Microsoft Edge** y damos Clic en la opción **Start without your data**

[![88-Microsoft-Edge.png](https://i.postimg.cc/3JbZRWkB/88-Microsoft-Edge.png)](https://postimg.cc/YhYFDrfG)

En las siguientes ventanas damos Clic en **Confirm and continue**, **Confirm and start browsing**

[![89-Confirm-browsing.png](https://i.postimg.cc/kGbzv5j3/89-Confirm-browsing.png)](https://postimg.cc/zHqxJqZ0)


- En el navegador ya configurado, ingresamos a nuestra cuenta de Azure Portal,

- Dentro de la Maquina Virtual ingresamos al disco local **C:** y creamos los siguientes directorios **proyect ** dentro otra carpeta **data** dentro otra carpeta **retail**.

- Dentro de la carpeta **retail** cargamos todos los archivos csv que tenemos en este repositorio de git de la carpeta **data**

[![89-copiar-csv.png](https://i.postimg.cc/sx1Q6vqX/89-copiar-csv.png)](https://postimg.cc/N2qjLfgq)

- En el navegador ya configurado, ingresamos a nuestra cuenta de Azure Portal,

- Dentro de la Maquina Virtual ingresamos al disco local **C:** y creamos los siguientes directorios **proyect ** dentro otra carpeta **data** dentro otra carpeta **retail**.

- Dentro de la carpeta **retail** cargamos todos los archivos csv que tenemos en este repositorio de git de la carpeta **data**

[![89-copiar-csv.png](https://i.postimg.cc/sx1Q6vqX/89-copiar-csv.png)](https://postimg.cc/N2qjLfgq)

Ingresamos a nuestra cuenta de Azure Portal con el navegador de nuestra VM, nos dirigimos a nuestro **Data Factory** ingresamos al **Launch studio**, nos dirigimos a **Manage**  y luego a **Integration runtimes** y damos Clic en **New** y seleccionamos **Azure Selft-Hosted** y le damos en **Continue**

[![90-Azure-Selft-Hosted.png](https://i.postimg.cc/nzcS29bg/90-Azure-Selft-Hosted.png)](https://postimg.cc/06FC2QN0)

[![91-Continue.png](https://i.postimg.cc/SN03FsW2/91-Continue.png)](https://postimg.cc/NL4pRQCB)

Damos Clic en **Continue** y **Name** colocamos **IR-LOCALHOST**

[![92-IR-LOCALHOST.png](https://i.postimg.cc/mrBfCrd1/92-IR-LOCALHOST.png)](https://postimg.cc/RWsDzvxv)

Damos Clic en **Create**

Nos ubicamos en la segunda opción Damos Clic en **Download and install integration runtime**

[![93-ir-localhost-setup.png](https://i.postimg.cc/x87fGb83/93-ir-localhost-setup.png)](https://postimg.cc/0KY1Pjpz)


En nuestro navegador le damos Clic a la opción **Download** y seleccionamos la segunda opción del **integration runtime**, y damos en la opción **Download**

[![94-opcion-IR.png](https://i.postimg.cc/2jgkcMCk/94-opcion-IR.png)](https://postimg.cc/Hj41jSJR)

Instalamos el  **integration runtime** que se ha descargado, dando **next** al botón de instalación, aceptando los términos y condiciones.

[![95-instalacion-integration-runtime.png](https://i.postimg.cc/Y091XD6w/95-instalacion-integration-runtime.png)](https://postimg.cc/njyjrkSd)

Llegaremos a la siguiente opción al darle Clic al botón **finish** de la instalación, **Register Integration Runtime (Selft-hested)**, acá es donde debemos copiar la ** key1** de **Authentication key***, tener en cuenta que la key1 es para un entorno productivo y la key2 es para un entorno de desarrollo.

[![96-register-integration-runtime.png](https://i.postimg.cc/4NfKrvfF/96-register-integration-runtime.png)](https://postimg.cc/z3QXhgRn)

[![93-ir-localhost-setup.png](https://i.postimg.cc/x87fGb83/93-ir-localhost-setup.png)](https://postimg.cc/0KY1Pjpz)

- Copiamos la **key1**
- Ponemos el check en **Show Authentication key**
- Clic en botón **Register**

[![96-register-integration-runtime-v2.png](https://i.postimg.cc/9fBGrs5P/96-register-integration-runtime-v2.png)](https://postimg.cc/KKRKWqY4)

Verificamos que nuestro **Selft Hosted** se haya registrado de manera correcta y finalmente le damos Clic en el botón **Finish**

[![97-verificacion-selft-hosted.png](https://i.postimg.cc/T11yRqbf/97-verificacion-selft-hosted.png)](https://postimg.cc/7JvP9zXW)

Cerramos la ventana dando Clic en el botón**Close**

[![98-boton-close.png](https://i.postimg.cc/bJDd1qzk/98-boton-close.png)](https://postimg.cc/gXpYWFL2)

Nos dirigimos al **CMD** de nuestra VM e ingresamos como administrador, nos ubicamos en la raiz del disco **C:/**

		cd ..

[![99-cmd.png](https://i.postimg.cc/KjQjtfK3/99-cmd.png)](https://postimg.cc/vxgGsWtG)

--

Ingresamos a la siguiente ruta: 

	cd "Program Files"
	cd "Microsoft Integration Runtime"
	cd "5.0"
	cd "Shared"

[![99-comandos-cmd.png](https://i.postimg.cc/FFLFV9Ss/99-comandos-cmd.png)](https://postimg.cc/yW7zVCnw)

Acá colocamos el comando que nos permitirá llevar nuestra información de la **VM Local ** a la parte de la nube de Azure.

Copiamos el siguiente comando en la CMD y damos **Enter**:

		.\dmgcmd.exe -DisableLocalFolderPathValidation

## CREACIÓN DE LINKED SERVICE

Nos dirigimos a nuestro **Azure Data Factory**

- Manage 
- linked service
- New
- File system

[![100-file-system.png](https://i.postimg.cc/k5ZG7sz7/100-file-system.png)](https://postimg.cc/v1rbzrGK)

Crear linked service para equipo local **(Virtual Machine)**

**Name: ** lkdsFileSystem
**Connect via integration runtime: ** seleccionamos IR-localhost
**Host: ** coiamos los directorios que creamos en nuestra VM **c:\project_azure_data***
**User name: ** nos vamos a nuestro recurso de la VM y buscamos la opción **connect ** y copiamos la opción Admin username: **vm-retail-prod@vm-retail-jcmd-001**
**Password: ** copiamos el password que ingresamos al momento de crear la VM


Clic en **Create**

[![101-create-lk-services-filessystem.png](https://i.postimg.cc/xC3dLQq1/101-create-lk-services-filessystem.png)](https://postimg.cc/phyxtgDw)

Regresamos a la sección de “Home”en nuestro Data Factory y hacemos clic en **“Ingest”**

[![102-Ingest-Data-D.png](https://i.postimg.cc/KvXhvHRQ/102-Ingest-Data-D.png)](https://postimg.cc/yWTbLnhS)

Configurar modo de ejecución

[![103-copy-data-tool.png](https://i.postimg.cc/PfQk6M6q/103-copy-data-tool.png)](https://postimg.cc/Z0ngn6zk)

Configurar fuente de datos

[![105-lks-configuration-datos.png](https://i.postimg.cc/J0YM4bdJ/105-lks-configuration-datos.png)](https://postimg.cc/gLhfg6pk)

Configurar tipo de archivo

[![106-file-format-setting.png](https://i.postimg.cc/XYzLcXt8/106-file-format-setting.png)](https://postimg.cc/xXLL9jfJ)

Configurar dataset

[![107-destination-data-store.png](https://i.postimg.cc/MTPpv2nv/107-destination-data-store.png)](https://postimg.cc/Vrt8VhB8)

Configurar formato de archivo

[![108-configuration-data-tool.png](https://i.postimg.cc/bY42Tkvd/108-configuration-data-tool.png)](https://postimg.cc/5jq09HLM)

Configurar nombre de pipeline

[![109-settings-copy-data-tool.png](https://i.postimg.cc/ncYcDDjy/109-settings-copy-data-tool.png)](https://postimg.cc/GHtC09jK)

Realizar despliegue de conexión

[![110-despliegue.png](https://i.postimg.cc/y607MZPD/110-despliegue.png)](https://postimg.cc/K4vXL4Cb)

Validar despliegue

[![111-validar-despliegue.png](https://i.postimg.cc/656FM21J/111-validar-despliegue.png)](https://postimg.cc/14TWttQJ)

Regresamos a **“Author”**, seleccionamos nuestro pipeline “PIPELINE_001_RETAIL” y ejecutamos

[![112-ejecucion-pipeline.png](https://i.postimg.cc/pdSNg24V/112-ejecucion-pipeline.png)](https://postimg.cc/WD0fZPsx)

Regresamos a nuestro “ADLS”container **“project”** y validamos la data

[![113-ADLS-landing.png](https://i.postimg.cc/rFvkpG9G/113-ADLS-landing.png)](https://postimg.cc/ZCFg7d20)


------------

## CREACIÓN DE ORQUESTACIÓN DE PIPELINES

Creamos un nuevo pipeline:

**Name: ** PIPELINE_004_MASTER_RETAIL

Nos ubicamos en la pestaña  **Genereal de Activities** y arrastramos un **Execute Pipeline**

**Name: ** EXC_INGEST_RETAIL

[![116-PIPELINE-004-MASTER-RETAIL.png](https://i.postimg.cc/VvkKr3Zh/116-PIPELINE-004-MASTER-RETAIL.png)](https://postimg.cc/G86F6SCP)

En la sección de **Settings** seleccionamos el **PIPELINE_001_INGEST_RETAIL**, luego arrastramos al lienzo un **Notebook** de **Databricks** de la sección de **Activities** y procedemos a unir el **Execute Pipeline** con el  **Notebook**

**Name: ** NTB_MAIN_RETAIL

En la pestaña de **Azure Databricks** seleccionamos el **lkdsDatabricks**

[![117-Notebook.png](https://i.postimg.cc/kXQ4HXQN/117-Notebook.png)](https://postimg.cc/RN046mbq)

En la pestaña de **Settings** ubicamos el notebook **Main** dentro de nuestro cluster de **Databricks** y damos Clic en Ok

[![118-NTB-MAIN-RETAIL.png](https://i.postimg.cc/rpb6N3p0/118-NTB-MAIN-RETAIL.png)](https://postimg.cc/sBp0Vn7s)


------------


# AGREGAMOS TRIGGER PARA LA ORQUESTACIÓN

Nos dirigimos a la sección **Trigger** y damos Clic en **New**

[![119-NEW-TRIGGER.png](https://i.postimg.cc/yY9F1ZRw/119-NEW-TRIGGER.png)](https://postimg.cc/PPXLKN24)

- **Name: ** TRIGGER_003_RETAIL
- **Recurrence Every: ** 1 - Week(s)
- Seleccionamos los días de Lunes a Viernes y colocamos 7 am su ejecución.

Damos Clic en **Ok**

[![120-NEW-TRIGGER-create.png](https://i.postimg.cc/rs2wQKpr/120-NEW-TRIGGER-create.png)](https://postimg.cc/wyFzBxLq)


------------

## CONEXIÓN CON POWER BI - CAPA DE VISUALIZACIÓN

Abrir Power BI y nos dirigimos al menú de opciones de **Inicio** --> **Obtener datos** --> **Más**

[![121-conexion-powerbi.png](https://i.postimg.cc/d1CTpHW9/121-conexion-powerbi.png)](https://postimg.cc/TLRwW9vy)

En la ventana emergente seleccionar **Azure** y **Azure SQL Database**

[![122-conexion-powerbi-azure.png](https://i.postimg.cc/1tmnvGgW/122-conexion-powerbi-azure.png)](https://postimg.cc/f3PRL0fX)

En la ventana emergente escribir los datos del servidor y seleccionar en modo de conexión **Importar**

[![123-conexion-powerbi-azure-bd.png](https://i.postimg.cc/DzvJfQ2q/123-conexion-powerbi-azure-bd.png)](https://postimg.cc/75RYm2J6)

En la ventana emergente seleccionar la **tabla** a **importar** y damos Click al botón Cargar

[![124-carga-datos.png](https://i.postimg.cc/kGcyjQvw/124-carga-datos.png)](https://postimg.cc/WqdrzJKJ)

## Presentación de Final de Visualización

[Abrir Gráfico Power BI](https://app.powerbi.com/view?r=eyJrIjoiY2E4NTg5NGEtYjlkNy00YmJmLWEwZTgtOTY2Y2YyMTFjOWE4IiwidCI6ImFlNWFkYzNmLWI2MDUtNGRjMy04NmM3LWM5NDgzNjE2MDk3MiJ9 "Abrir Gráfico Power BI")

##### Descripción General del Gráfico

En este gráfico se proporciona una visión clara y concisa del rendimiento general de las ventas de la empresa, mostrando tanto los totales de ventas mensuales como las cifras clave acumuladas.

En este gráfico se destacan las categorías y productos más vendidos, así como los departamentos con mejores desempeños, permitiendo a la empresa identificar áreas clave de éxito y posibles áreas de mejora.

[![125-gr-fico-PB1-1.png](https://i.postimg.cc/NGbt2L3k/125-gr-fico-PB1-1.png)](https://postimg.cc/23qMPj81)

##### Descripción General del Gráfico

El gráfico presentado proporciona una visión detallada de las ventas, centrada en varios aspectos clave de la actividad comercial de la empresa. Entre las principales que se pueden extraer estan:

- **Clientes Clave:** Identificar los clientes principales puede ayudar a la empresa a personalizar estrategias de marketing y programas de fidelización para estos clientes, dado su impacto significativo en las ventas totales.

- **Categorías Predominantes:** Las categorías como **"Fishing", "Cleats" y "Camping & Hiking"** son altamente lucrativas y deberían recibir mayor atención en términos de inventario y promociones.

- **Diferencias Regionales:** Las categorías más vendidas varían por ciudad, lo que sugiere que las preferencias de los clientes son específicas a las regiones. Esto puede ser útil para diseñar estrategias de ventas y marketing localizadas.

- **Productos Estrella:** Los productos como el **"Field & Stream Sportsman 16 Gun Fire Safe"** destacan en varias ciudades, indicando su alta popularidad y demanda. La empresa debería asegurar un buen suministro de estos productos y considerar promociones adicionales para maximizar las ventas.

[![126-gr-fico-PB2.png](https://i.postimg.cc/3JBzb68j/126-gr-fico-PB2.png)](https://postimg.cc/3dkLktDW)

## Conclusiones y Mejores Prácticas

Este proyecto demuestra cómo los servicios de Azure pueden integrarse de manera efectiva para crear una solución robusta de análisis de datos. Siguiendo las mejores prácticas de seguridad, automatización y escalabilidad, es posible construir una infraestructura eficiente y segura que soporte el análisis de grandes volúmenes de datos.

## Mejores Prácticas

- Seguridad: Implementar políticas de seguridad estrictas y utilizar servicios como Azure Key Vault para la gestión de secretos asegura que los datos estén protegidos en todo momento.

- Escalabilidad: Aprovechar las capacidades de escalabilidad de Azure permite manejar grandes volúmenes de datos y ajustar los recursos según las necesidades del proyecto.

- Monitoreo y Mantenimiento: Configurar alertas y monitoreo continuo de los servicios permite identificar y resolver problemas rápidamente, garantizando la alta disponibilidad y rendimiento del sistema.

## Recursos Adicionales

Para obtener más información sobre los servicios utilizados en este proyecto, consulta los siguientes recursos:

- [Documentación de Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/ "Documentación de Azure Synapse Analytics")

- [Guía de Azure Data Factory](https://learn.microsoft.com/en-us/azure/data-factory/ "Guía de Azure Data Factory")

- [Documentación de Azure SQL Database](https://learn.microsoft.com/en-us/azure/azure-sql/?view=azuresql "Documentación de Azure SQL Database")

- [Tutorial de Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/ "Tutorial de Azure Key Vault")

- [Documentación de Azure Databricks](https://learn.microsoft.com/en-us/azure/databricks/ "Documentación de Azure Databricks")

- [Guía de Power BI](https://learn.microsoft.com/es-es/power-bi/ "Guía de Power BI")
