> Las herramientas de Microsoft ASP.NET para Azure Active Directory facilitan la autenticación para aplicaciones web alojadas en [Azure] (https://www.windowsazure.com/en-us/home/features/web-sites/). Puede utilizar la autenticación de Azure para autenticar los usuarios de Office 365 de su organización, las cuentas corporativas sincronizadas de su Active Directory en el sitio o los usuarios creados en su propio dominio Azure Active Directory personalizado. La habilitación de la autenticación de Windows Azure configura su aplicación para autenticar a los usuarios utilizando un inquilino único [Azure Active Directory] (https://www.windowsazure.com/en-us/home/features/identity/).
> 

Este tutorial te enseñanara como crear una aplicacion ASP.NET que esta configurada para autenticacion Azure AD. [Azure Active Directory](https://msdn.microsoft.com/en-us/library/azure/mt168838.aspx) (Azure AD).

## Prerequisitos

1. Visual Studio 2015 Update 3
2. Una cuenta en Azure. [Clic aqui](https://azure.microsoft.com/en-us/pricing/free-trial/) 

## Agregar un administrador global al directorio

1. Logearse al portal moderno [Azure  Portal](https://portal.azure.com/).
2. Todas las cuentas de Azure tienen un Directorio por defecto buscar directorio activo en el menu de la izquierda.
![](/content/images/2017/02/EntrarAAzureActiveDirectory.png)
3. Clic Add User.  
4. Crear un usuario nuevo con el rol de Administrador Global.  
![](/content/images/2017/02/CrearUsuario.png)
5.Tome nota del password para utilizar luego
  
   
Guarde la contraseña, se le pedirá que cambie la contraseña después del primer inicio de sesión.

## Crear una aplicacion ASP.NET

1. Crear una aplicacion de tipo ASP.NET
![](/images/2017/02/1.png)

2.Seleccionar MVC y Cambiar autenticacion, en autenticacion seleccionar cuentas de trabajo o estudiantes y automaticamente se llenar el nombre de su directorio activo, tambien seleccionar la casilla de Leer datos del directorio
![](/images/2017/02/2.png)

3.El proyecto ha sido creado, presione F5 para ver el resultado.
![](/images/2017/02/3.png)

4.En el portal de Azure, seleccionar App Registrations para ver como ha quedado registrado.
![](/images/2017/02/4.png)

5.Seleccionar la App registrada y verificar que la direccion de retorno este correcta.
![](/images/2017/02/5.png)

6.Entrar a la aplicacion con el usuario que es administrador global.
![](/images/2017/02/6.png)

7.Luego se debe consentir que la App nos esta pidiendo permisos para leer datos del directorio.
![](/images/2017/02/7.png)

8.Despues de estar logeados podemos ver nuestra cuenta en la parte superior.
![](/images/2017/02/8.png)