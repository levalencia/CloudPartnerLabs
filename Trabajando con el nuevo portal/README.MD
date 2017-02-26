Introduccion al Portal de Azure
=============================================

El nuevo portal de Azure es un todo-en-uno, trabajo-dondequiera experiencia. Ahora puede administrar App Service Apps, bases de datos y proyectos de Visual Studio Team Services en un UX reimaginado personalizado a su estilo de trabajo. Fue construido desde cero para poner sus _applications_ en el centro de la experiencia.

Este centro unificado simplifica radicalmente la creación, implementación y administración de los recursos de la nube. Imagine una consola única y fácil de usar construida solo para usted, su equipo, sus proyectos. Reúne todos los recursos de la nube, los miembros del equipo y las etapas del ciclo de vida de su aplicación y le proporciona un lugar centralizado para planificar, desarrollar, probar, proporcionar, implementar, escalar y supervisar esas aplicaciones. Este enfoque puede ayudar a los equipos a adoptar una cultura DevOps al reunir capacidades y perspectivas de desarrollo y operaciones de una manera significativa.

El nuevo portal permite a cada usuario transformar la página principal del portal (denominada el Panel de control) en su propio panel personalizado. Manténgase al tanto de las cosas que más importan fijándolas a su ** Dashboard **. Re-tamaño de las piezas para mostrar más o menos datos. Perfore todos los detalles. Y vea insights (y oportunidades) entre aplicaciones y recursos. Los nuevos componentes incluyen lo siguiente:

* **Gestión de Recursos Simplificada**. En lugar de administrar recursos independientes como Azure App Service Web Apps, Proyectos de Visual Studio o bases de datos, los clientes pueden ahora crear, administrar y analizar toda su aplicación como un solo grupo de recursos en una experiencia unificada y personalizada. Hoy en día, el nuevo gestor de Azure también está disponible a través del último SDK de Azure para que los clientes puedan automatizar su implementación y administración desde cualquier cliente o dispositivo.

* **Facturación integrada**. Una nueva experiencia de facturación integrada permite a los desarrolladores y profesionales de TI tomar el control de sus costes y optimizar sus recursos para obtener la máxima ventaja comercial.

* **Marketplace**. Un mercado rico de aplicaciones y servicios de Microsoft y la comunidad de código abierto. Este mercado integrado de servicios gratuitos y de pago permite a los clientes aprovechar el ecosistema para ser más ágil y productivo.

* **Visual Studio Team Services**. Microsoft anunció mejoras clave a través del portal de Microsoft Azure. Esto incluye Team Projects que soportan una mayor agilidad para la gestión del ciclo de vida de las aplicaciones y el editor ligero denominado "Monaco" para modificar y comprometer los cambios de código del proyecto Web sin salir de Azure. También se incluye Application Insights, una solución analítica que recopila datos de telemetría como disponibilidad, rendimiento e información de uso para rastrear la salud de una aplicación. La integración de Visual Studio permite a los desarrolladores exponer estos datos desde nuevas aplicaciones con un solo clic.

## Creando una aplicacion Web + SQL ##

Históricamente, administrar un recurso (una entidad gestionada por el usuario, como un servidor de base de datos, una base de datos o una aplicación Web) en Microsoft Azure requería que realizaran operaciones contra un recurso a la vez. Cuando desarrollamos para la nube hoy, a menudo estamos gestionando recursos_individuales (bases de datos, almacenamiento, servicios en la nube, máquinas virtuales, etc.). Nos queda a nosotros, como desarrolladores de nubes y profesionales de TI, juntar estos recursos de una manera significativa y gestionarlos con el tiempo. El portal de Microsoft Azure fue diseñado para reunir todos los recursos individuales de una aplicación en una vista consolidada. Grupo de recursos es un nuevo concepto en Azure que sirve como límite de ciclo de vida para todos sus recursos.

En esta tarea, aprenderá sobre el portal y cómo crear una nueva aplicación web y una base de datos SQL que lo utilice.

 1. Abra un navegador y vaya a http://portal.azure.com e inicie sesión usando sus credenciales.

 2. TLo primero que verás es el ** Dashboard **. Esta es su página de inicio, donde puede ver datos dinámicos de sus recursos y todos los detalles que le interesan. Usted puede modificarlo para requisitos particulares como usted crea.

	> **Nota:** Puede hacer clic con el botón derecho en los cuadros del panel para personalizarlo. Puede fijar o desenrollar las baldosas y cambiar su tamaño.

	![Startboard](Images/startboard.png?raw=true)

	_Su pagina inicial: El Dashboard_

 3. En el lado izquierdo, usted vera el  **Hub Menu**. Este es el menu de navegacion donde podras acceder a todos sus recursos y opciones. Clic el boton **New** en la parte superior del  **Hub Menu**.

	![Creating a new resource](Images/creating-a-new-resource.png?raw=true)

	_Creando un nuevo recurso_

 4. Se muestra un panel con diferentes opciones. Puede elegir una de las opciones para crear un nuevo recurso. Clic la categoria  **Web + mobile** para ver todas las opciones disponibles.

	![Displaying every resource type](Images/displaying-resource-types.png?raw=true)

	_Mostrando todas las opciones disponibles_

	En la siguiente pantalla, obtendrá todas las opciones disponibles para Web, aplicaciones para móviles y otras aplicaciones de servicio de aplicaciones de Azure. Haga clic en **See all**. Puede encontrar plantillas listas para usar.

	![Azure Marketplace](Images/web-mobile-apps-options.png?raw=true)

	_Azure Marketplace_


 5. Encuentre la opcion **Web app + SQL** y luego haga clic.

	![Selecting Web App](Images/selecting-website-sql.png?raw=true)

	_Seleccionando Web + SQL_

	Luego clic **Create**.

	![Creating Web app](Images/create-web-app-sql.png?raw=true)

	_Creando la aplicacion web + SQL_

	Un _blade_ es abierto. Los Blades son su punto de entrada para descubrir ideas, realizar acciones y crear aplicaciones. Este _blade_ particular recoge la entrada de usted para crear una nueva **Web App**.

	> **Nota:** Para obtener más información sobre _blades_, puede hacer clic en el blade **Tour**  en su **Dashboard**. En la hoja **Tour**, desplácese hasta la parte inferior y haga clic en **Learn more**. Se abre una nueva hoja con más información. Puede continuar el **Tour** para aprender los fundamentos de Blades, Comandos, Lentes y más.

	> ![Tour](Images/tour.png?raw=true)

 6. Seleccione la opcion *App Service Name* , otro  blade es abierto que muestra las opciones para crear un nuevo **Web app**. Seleccione una URL para su Web App, por ejemplo, for example _myMSAzureWebsite_. Tenga en cuenta que este nombre debe ser unico. Tambien seleccione **Create new App Service Plan** o  **Select existing** si ya tienes uno.

	![Creating the App Service Plan](Images/changing-the-web-hosting-plan.png?raw=true)

	_Creando un nuevo App Service Plan_

 7. Si desea cambiar el nivel de precios, haga clic en **Pricing Tier**.  En la opción  _Choose your pricing tier_, puede elegir el plan de App Service que se adapte a sus necesidades..

	Los App Service Plan representan un conjunto de funciones y capacidad que puede compartir en todas las App Service Plan, incluidas las aplicaciones web. Los App Service Plan admiten varios niveles de precios (por ejemplo, gratuito, compartido, básico, estándar y Premium), cada uno con sus propias capacidades. Hay un par de diferencias entre estos niveles. Los planes en el nivel Libre y Compartido proporcionan a los sitios una infraestructura compartida, lo que significa que sus sitios comparten recursos con los sitios de otros clientes. App Service Plan en los sitios de  de niveles básicos, estándar y Premium con una infraestructura dedicada, lo que significa que sólo se aplicarán a dichos recursos la aplicación o las aplicaciones que elija asociar a este plan. En este nivel, puede configurar su App Service Plan para utilizar una o más instancias de la máquina virtual. Como vamos a utilizar el App Service Plan predeterminado, sólo cierre este blade.

	> **Nota:** Para todas (excepto 'Shared') Usted paga un precio por el App Service Plan basado en el nivel y la capacidad elegida sin ningún cargo adicional por cada sitio que utiliza el plan. Los planes de servicio de aplicación compartida son diferentes; Debido a la naturaleza de la infraestructura compartida, se le cobrará por separado para cada aplicación del plan. Haga clic en _Select_ una vez que haya seleccionado un plan relevante. Esto debería llevarlo a la hoja anterior para las opciones _Web App_.

	![_Selecting a Pricing Tier](Images/selecting-a-web-hosting-plan.png?raw=true)

	_Seleccione un Precio_

 8. Seleccione una ubicación relevante y haga clic en Aceptar.

 9. Clic **Database** para cambiar la configuracion de la Base de Datos.

	> **Nota:** Si hay bases de datos asociadas al usuario, el blade de base de datos aparecera, seleccione **Create a new Database**.

	![Changing your database settings](Images/changing-your-database-settings.png?raw=true)

	_Cambiar la configuracion de base de datos_

 10. Defina un nombre para la base de datos, e.g. _mywebsite-db_, y haga  clic en la opcion **Server**.

	> **Nota:** Ud tambien puede revisar la opcion **Pricing Tier** y explorar las diferentes opciones de precios.

	![Database Settings](Images/database-settings.png?raw=true)

	_Configuracion de base de datos_

 11. Seleccione **Server Name**, **Server admin login** y un **Password**. Clic **OK** para regresar al blade **New Database**, y clic **OK** para cerrarlo.

	![Configuring the Database Server](Images/configuring-the-database-server.png?raw=true)

	_Configuracion del servidor de base de datos_
	
 12. Cuando crea una aplicación que consta de varios recursos trabajando juntos (como en este ejemplo, una aplicación Web + SQL), siempre se crea en su propio grupo de recursos para poder administrar el ciclo de vida de todos los activos relacionados. Elija un nombre para el **Resource Group**, por ejemplo _MyResourceGroup_, y haga clic en la opción **Web app**.

	> **Nota:** Los nombres de grupos de recursos sólo pueden contener letras, números, puntos, subrayados y guiones.

	![New Web App](Images/new-resource-group.png?raw=true)

	_Nuevo grupo de recursos_

 13. Ahora ya está listo para crear su grupo de recursos. Hacer clic **Create**.

 14. En la parte superior derecha, haga clic en el icono de campana para **Notifications**. Puede ver el estado del proceso de creación en la sección **Notifications**. 

	![Notifications](Images/notifications.png?raw=true)

	_Notificaciones_

 15. Una vez creado el grupo de recursos, el mensaje de notificación muestra el mensaje correspondiente.
	
	![Created Resource Group Notification](Images/created-resource-group-notification.png?raw=true)

	_Notificación de grupo de recursos creada_
	
 16. Observe que el grupo de recursos creado recientemente se agregó automáticamente al panel para facilitar el acceso. 

	![Resource Group added to Dashboard](Images/resource-group-added-to-startboard.png?raw=true)

 17. Haga clic en el nombre del grupo de recursos para abrirlo. Ha creado su nuevo grupo de recursos, que incluye una aplicación Web, un servidor SQL Server y una base de datos SQL.

	![New Resource Group Blade](Images/new-resource-group-blade.png?raw=true)

	_Blade Nuevo Grupo de recursos_

	>**Nota:** Es una buena práctica limpiar cualquier recurso que no se utilice. Para eliminar los recursos de demostración creados, siga estos pasos:
	>
	>1. En el Resource Group, clic  **DELETE** en la parte superior de la pagina.
	>
	>![Clicking Delete Resource Group](Images/clicking-delete-resource-group.png?raw=true)
	>
	>_Clic Delete_
	>
	>1. En el blade de confirmacion, digite el nombre del grupo y luego clic en Delete.
	>
	>![Deleting Resource Group Confirmation](Images/deleting-resource-group-confirmation.png?raw=true)
	>
	>_Confirmacion de borrado_
	>
	>Después de unos momentos, verá una notificación en el Centro de notificaciones indicando que se ha eliminado el grupo de recursos.
	>
	>![Notification after Resource Group was deleted](Images/notification-after-resource-group-was-deleted.png?raw=true)
	>
	>1. Vaya al  Dashboard. Una vez ahi, encuentre el blade del grupo de recursos, dale clic derecho y seleccione Unpin para quitarlo del dashboard.
	>
	>![Unpin Resource Group from Dashboard](Images/unpin-resource-group-from-startboard.png?raw=true)
	>
	>_Unpinning Resource Group from DashBoard_
	>El blade es eliminado.


<a name="Summary"></a>
## Summary ##

El nuevo portal de Azure ofrece una mirada emocionante sobre el futuro de DevOps. Esta es una experiencia de primera clase que reúne todos los recursos individuales, las personas y las etapas del ciclo de vida de su aplicación en un portal unificado. En este laboratorio, aprendió sobre el portal y cómo crear un nuevo grupo de recursos mediante la creación de una aplicación web.