# Sistemas-distribuidos-II
Proyecto venta de productos:
Objetivo 
El objetivo general del proyecto final del curso ISD se centra en la apropiación práctica de los conceptos relacionados con el desarrollo de aplicaciones y la utilización de middlewares en ambientes de cómputo distribuido. Este propósito se logra mediante la creación de un sistema distribuido que brinde una solución a una problemática del mundo real. Los objetivos específicos son: 

• Poner en práctica los conceptos de arquitecturas distribuidas, haciendo énfasis en algunas de las problemáticas clave de gestión de la concurrencia: gestión de archivos, control de concurrencia, manejo transaccional y recuperación ante fallas. 

• Implementar, mediante el uso de RMI, un sistema no centralizado que logre la cooperación entre procesos mediante la definición de protocolos de interacción bien definidos. 

Para este período académico, el proyecto se focalizará en la gestión de tarjetas débito sociales para gestionar ayudas a familias en situación de vulnerabilidad. Todos los mecanismos de gestión de transacciones del sistema deben funcionar en forma distribuida y garantizando el cumplimiento de las propiedades ACID. Todas las comunicaciones entre procesos deben hacerse por medio del middleware RMI de Java. 

Descripción del Sistema a Desarrollar 
El proyecto consiste en desarrollar e implementar una aplicación distribuida para apoyar al gobierno nacional en la gestión de una tarjeta débito de consumo para población en condición de vulnerabilidad, en donde se consigna mensualmente una cantidad determinada de dinero. Los beneficiarios de la tarjeta pueden acceder a una tienda virtual en la que podrá hacer compras a precios especiales. 
Un usuario cliente, para interactuar con la máquina que tiene la lógica de la tienda virtual contacta siempre vía RMI a dos servidores que manejan de manera consistente los objetos involucrados en las transacciones. El servidor del catálogo de productos maneja toda la información de los productos que hay en la tienda virtual, así como información transaccional cuando uno o más usuarios ingresan a la plataforma. Pueden presentarse inconsistencias, cuando de forma concurrente varios clientes piden el mismo producto. También puede haber inconsistencias si en forma concurrente con alguna compra se hace el ingreso de nuevos productos al inventario. 

Al inicio el cliente abre su sesión identificándose con el número de tarjeta, a continuación, se seleccionan productos y cantidades; finalmente, al confirmar el pedido, se inicia una transacción. En la transacción, en primer lugar, se verifica que se cuenta con saldo suficiente para el pago total del 
pedido. 
En caso afirmativo, se procede a hacer la afectación en el inventario uno a uno de los productos del pedido; antes de hacer la modificación de cada ítem debe verificarse que hay la cantidad suficiente de productos solicitados. Si no hay existencias suficientes de un ítem, éste es eliminado del pedido y se informa al usuario; los demás ítems se siguen procesando. Finalmente, se hace el descuento del dinero del saldo asociado a la tarjeta; en caso de que se hayan eliminado ítems, se debe hacer el ajuste respectivo. 

Adicionalmente, en el sistema hay un usuario administrador que puede hacer el ingreso de dinero a las tarjetas. De igual forma, el administrador puede hacer el ingreso al inventario de nuevos productos. Estas acciones deben tener manejo transaccional. Se podría dar el caso de que justo en el momento en que se está realizando una compra, ingresa dinero a la cuenta o nuevos productos. 

Para mayor facilidad, tanto el catálogo de productos como la lista de tarjetas habilitadas son fijas. Al arranque de los servidores, se cargan estos datos desde un archivo tipo texto; para cada tarjeta se inicializa el saldo y para cada producto se fija el inventario inicial. El servidor para la gestión de los usuarios, que incluye el manejo de las tarjetas y saldos opera en una máquina independiente; y el servidor para la gestión del catálogo de productos y el inventario corre en otra máquina independiente. Los usuarios, tanto clientes como administrador, se conectan desde máquinas diferentes. 

Con el fin de hacer más robusto al sistema, como un componente “opcional” se puede incluir el manejo del control de acceso. La primera vez que un cliente ingresa al sistema, debe generar una contraseña nueva, se genera el hash correspondiente y se almacena de forma permanente, con el fin de hacer validaciones posteriores. Para asegurar la confidencialidad de las contraseñas se debe utilizar cifrado. 
Comunicación Distribuida 
Teniendo en cuenta que en una transacción normalmente se trabajan múltiples productos y que pueden existir múltiples transacciones simultaneas, además usando servidores en máquinas diferentes, es probable que se presenten problemas transaccionales. Por lo tanto, se deben utilizar el protocolo de consumación atómica de dos fases 2PC para asegurar la consistencia cumpliendo con las propiedades ACID. De igual forma, es posible que se presenten problemas de concurrencia; por lo tanto, se debe utilizar un protocolo de control optimista de concurrencia con validación hacia adelante. 

Para la gestión transaccional se deben integrar en forma coordinada la gestión del 2PC con la del control de concurrencia. Se deben gestionar todas las copias tentativas de los objetos que están siendo utilizados por los diferentes usuarios. El estado del inventario de los productos y de los saldos de las tarjetas debe quedar guardado de forma permanente cuando se hace commit de las transacciones.

El sistema debe incluir mecanismos de recuperación ante fallas. Cuando uno de los servidores deja de funcionar y posteriormente entra en operación, la reconexión debe hacerse de forma automática, es decir, será transparente para los usuarios; las transacciones deben continuar en el punto en que se habían dejado y no iniciar desde el principio. Durante el desarrollo de las transacciones debe registrarse en disco información que permita realizar la recuperación del servidor de catálogo de productos. Se deja como tarea adicional “opcional” incluir la recuperación ante fallas para el servidor de usuarios. 

Todas las interacciones entre los procesos asociados a los servidores y usuarios deben realizarse mediante el uso de RMI. 

