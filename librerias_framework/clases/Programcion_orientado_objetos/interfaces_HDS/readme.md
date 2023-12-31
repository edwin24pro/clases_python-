## averiguar de tkinter
## que es
## modo de uso
## ejemplo praactico --> main.py
# Tkinter
El paquete tkinter («interfaz Tk») es la interfaz por defecto de Python para el kit de herramientas de GUI Tk. Tanto Tk como tkinter están disponibles en la mayoría de las plataformas Unix, así como en sistemas Windows (Tk en sí no es parte de Python, es mantenido por ActiveState).

Ejecutar python -m tkinter desde la línea de comandos debería abrir una ventana que demuestre una interfaz Tk simple para saber si tkinter está instalado correctamente en su sistema. También muestra qué versión de Tcl/Tk está instalada para que pueda leer la documentación de Tcl/Tk específica de esa versión.

Tkinter soporta un amplio rango de versiones TCL/TK, construidos con o sin soporte de hilo. La versión oficial del binario de python incluye Tcl/Tk 8.6 con subprocesos. Vea el código fuente para el módulo _tkinter para mayor información acerca de las versiones soportadas.

Tkinter no es un subproceso liviano pero agrega una buena cantidad de su propia lógica para hacer la experiencia mas pythónico. Esta documentación se concentrará en estas adiciones y cambios, refiriéndose sobre la documentación oficial de Tcl-Tk para los detalles que no han sido cambiados.

Nota Tcl/Tk 8.5 (2007) introdujo un moderno conjunto de componentes temáticos de interfaz de usuario en conjunto con una nueva API para ser usada. Ambas API tanto la antigua como la nueva están disponibles. La mayor parte de la documentación que podrá encontrar en línea aún usa la antigua versión de la API y por lo tanto podría estar desactualizada.
Ver también
TkDocs
Es un extenso tutorial sobre como crear interfaces de usuario con Tkinter. Explica conceptos clave y muestra enfoques recomendados para usar la API moderna.

Tkinter 8.5 reference: a GUI for Python
Documentación de referencia sobre Tkinter 8.5 donde se detallan clases disponibles, métodos y opciones.

Recursos de Tcl/Tk:

Comandos Tk
Referencia exhaustiva para cada uno de los comandos subyacentes utilizados por Tkinter.

Tcl/Tk Home Page
Documentación adicional y enlaces al núcleo de desarrollo de Tcl/Tk.

Libros:

Modern Tkinter for Busy Python Developers
Escrito por Mark Roseman (ISBN 978-1999149567)

Python y la programación de Tkinter
Escrito por Alan Moore (ISBN 978-1788835886)

Programming Python
Escrito por Mark Lutz, que cubre los tópicos de Tkinter de forma excelente. (ISBN 978-0596158101)

Tcl and the Tk Toolkit
Escrito por John Ousterhout, creador de Tcl/Tk y Kevin Jones, el cual no cubre Tkinter. (ISBN 978-0321336330)

Arquitectura
Tcl/Tk no es una biblioteca única mas bien consiste en unos pocos módulos distintivos entre si. cada uno separados por funcionalidades y que poseen su documentación oficial. Las versiones del binario de Python también incluyen un módulo adicional con esto.

Tcl
Tcl es un lenguaje de programación interpretado y dinámico, tal como Python. Aunque puede ser usado en si mismo como un lenguaje de programación de uso general, lo más común es que se utilice en aplicaciones del lenguaje C como un motor de secuencias de comandos o como una interfaz hacia el kit de herramientas de Tk. La librería TCL contiene una interfaz con C que crea y gestiona una o más instancias del intérprete de TCL, corre los comandos en TCL en esas instancias y agrega comandos personalizados tanto en Tcl o C. Cada intérprete tiene su cola de eventos y entrega facilidades para enviar procesos a la cola y procesarlos. A diferencia de Python, el modelo de ejecución en Tcl es diseñado alrededor de cooperar en múltiples asignaciones y Tkinter ayuda a salvar estas diferencias (ver Threading model para más detalles).

Tk
Tk es un paquete de Tcl implementado en C que agrega comandos personalizados que permiten manipular widget en la GUI. Cada objeto de la clase Tk agrega su instancia de interprete Tcl con Tk cargado en el. Los widget de Tk son muy personalizables, aunque a costa de una apariencia anticuada. Tk usa eventos de la cola de Tcl para generar y procesar eventos de la GUI.

Ttk
El nuevo tema Tk (Ttk) es una nueva familia de widgets de Tk que proveen una mejor apariencia en diferentes plataformas mas que varios de los widgets clásicos de Tk. Ttk es distribuido como parte de Tk estando disponible a partir de la versión 8.5. Enlaces para Python son entregados en un módulo aparte, tkinter.ttk.

Internamente, Tk y Ttk usan funcionalidades del sistema operativo subyacente, en otras palabras, Xlib en Unix/X11, Cocoa en macOS o GDI en Windows.

Cuando su aplicación de Python una una clase en Tkinter, p.ej, para crear un widget, el módulo tkinter ensambla una cadena de comando Tcl/Tk. Entonces se pasa esa cadena de comando Tcl a un módulo binario interno _tkinter, que luego llama al intérprete Tcl para evaluarlo. El intérprete de Tcl llamará a los paquetes Tk y o Ttk, que a su vez realizarán llamadas a Xlib, Cocoa o GDI.

Módulos Tkinter
El soporte para Tkinter se distribuye para varios módulos. la mayor parte de las aplicaciones necesitaran el módulo base tkinter, así como también el módulo tkinter.ttk, el cual entrega el conjunto de widget temáticos y la API correspondiente:

from tkinter import *
from tkinter import ttk
class tkinter.Tk(screenName=None, baseName=None, className='Tk', useTk=True, sync=False, use=None)
Esto crea un widget de nivel superior de Tk que generalmente es la ventana principal de una aplicación. Cada instancia tiene su propio intérprete Tcl asociado.

La clase Tk normalmente se instancia utilizando los valores predeterminados. No obstante, se reconocen los siguientes argumentos de palabra clave:

screenName
Cuando se proporciona (como cadena de caracteres), establece la variable de entorno DISPLAY. (solo X11)

baseName
Nombre del archivo de perfil. De forma predeterminada, baseName se deriva del nombre del programa (sys.argv[0]).

className
Nombre de la clase del widget. Se utiliza como archivo de perfil y como nombre con el que se invoca a Tcl (argv0 en interp).

useTk
Si es True, inicializa el subsistema Tk. La función tkinter.Tcl() se establece en False.

sync
Si es True, ejecuta todos los comandos del servidor X de forma síncrona, por lo tanto, los errores se notifican inmediatamente. Se puede utilizar en el proceso de depuración. (solo X11)

use
Especifica el id de la ventana en la que se incrustar la aplicación, en lugar de crearla como una ventana independiente de nivel superior. El id debe especificarse de la misma manera que el valor para la opción -use de los widgets de nivel superior (es decir, emplea el formato de retorno de winfo_id()).

Hay que tener en cuenta que en algunas plataformas, esto solo funcionará correctamente si el id se refiere a un marco Tk o a un nivel superior que tenga habilitada la opción -container.

Tk reads and interprets profile files, named .className.tcl and .baseName.tcl, into the Tcl interpreter and calls exec() on the contents of .className.py and .baseName.py. The path for the profile files is the HOME environment variable or, if that isn’t defined, then os.curdir.

tk
El objeto de la aplicación Tk creado al instanciar Tk. Esto proporciona acceso al intérprete de Tcl. Todos los widget que se adjuntan a la misma instancia de Tk tienen el mismo valor para el atributo tk.

master
El objeto widget que contiene este widget. Para Tk, master es None, ya que es la ventana principal. Los términos master y parent son similares, y a veces se usan indistintamente como nombres de argumento; Sin embargo, al llamar a winfo_parent() este retorna una cadena con del nombre del widget, mientras que master retorna el objeto. parent/child refleja la relación en forma de árbol, mientras que master/slave, refleja la estructura del contenedor.

children
Los descendientes directos de este widget como dict con los nombres del widget secundario como clave y los objetos de instancia secundarios como valor.

tkinter.Tcl(screenName=None, baseName=None, className='Tk', useTk=False)
La función Tcl() es una función de fábrica que crea un objeto muy similar al creado por la clase Tk, excepto que no inicializa el subsistema Tk. Esto suele ser útil cuando se maneja el intérprete de Tcl en un entorno en el que no se desean crear ventanas de nivel superior extrañas o donde no se puede (como los sistemas Unix/Linux sin un servidor X). Un objeto creado por el objeto Tcl() puede tener una ventana Toplevel creada (y el subsistema Tk inicializado) llamando a su método loadtk().

Otros módulos que proporcionan soporte a Tk incluyen:

tkinter
Módulos de Tkinter.

tkinter.colorchooser
Cuadro de diálogo que permite al usuario elegir un color.

tkinter.commondialog
Clase base para cuadros de diálogo definidos en los otros módulos listados aquí.

tkinter.filedialog
Cuadros de diálogo por defecto que permiten al usuario especificar un archivo para abrir o guardar.

tkinter.font
Utilidades para ayudar a trabajar con fuentes.

tkinter.messagebox
Acceso a cuadros de diálogo estándar de Tk.

tkinter.scrolledtext
Widget de texto con una barra de desplazamiento vertical integrada.

tkinter.simpledialog
Cuadros de diálogo simples y funciones útiles.

tkinter.ttk
Un set de widgets temáticos fueron añadidos en Tk 8.5, entregando alternativas modernas para muchos de los widgets clásicos en el módulo principal de tkinter.

Módulos adicionales:

_tkinter
Un módulo binario que contiene una interfaz de bajo nivel con Tcl/Tk. . Esto es automáticamente importado al módulo principal tkinter y nunca debe ser usado directamente por los p programadores de aplicaciones. Esto es usualmente una librería compartida (or DLL), pero podría en ciertos casos ser añadidos de forma estática por el interprete de Python.

idlelib
Entorno de aprendizaje y desarrollo integrado de Python (IDLE). Basado en tkinter.

tkinter.constants
Constantes simbólicas que pueden ser utilizadas en en lugar de cadenas de texto cuando se pasan varios parámetros a las llamadas de Tkinter. Automáticamente son importadas por el módulo principal tkinter.

tkinter.dnd
De forma experimental está el soporte de arrastrar y soltar (drag-and-drop) para tkinter. Esto no se mantendrá cuando sea reemplazado por Tk DND.

tkinter.tix
(Obsoleto) Un antiguo paquete de terceros para utilizar en Tcl/Tk que agrega un importante número de nuevos widgets. Se pueden encontrar mejores alternativas en el módulo tkinter.ttk.

turtle
Gráficos de tortuga en una ventana Tk.

Guía de supervivencia de Tkinter
Esta sección no fue pensada en ser un tutorial exhaustivo sobre cada tópico de Tk o Tkinter. Para esto, puede consultar en alguno de los recursos externos que fueron presentados anteriormente. En vez de eso, esta apartado entrega una orientación breve sobre como es ve una aplicación en Tkinter, identifica conceptos claves de Tk y explica como el empaquetador de Tkinter está estructurado.

Es resto de esta sección lo ayudará a identificar las clases, métodos y opciones que necesitará en una aplicación de Tkinter y donde se puede encontrar documentación más detallada sobre esto, incluida en la guía de referencia oficial de Tcl/Tk.

Un programa simple de Hola Mundo
Comenzaremos a explorar Tkinter a través de una simple aplicación «Hola Mundo». Esta no es la aplicación mas pequeña que podríamos escribir, pero es suficiente para ilustrar algunos conceptos claves que necesitará saber.

from tkinter import *
from tkinter import ttk
root = Tk()
frm = ttk.Frame(root, padding=10)
frm.grid()
ttk.Label(frm, text="Hello World!").grid(column=0, row=0)
ttk.Button(frm, text="Quit", command=root.destroy).grid(column=1, row=0)
root.mainloop()
Después de las declaraciones de import, la línea siguiente será la de crear la instancia de la clase Tk la cual iniciativa Tk y crea el interprete asociado a Tcl. Esto también crea una ventana de nivel superior, conocida como la ventana raíz la cual sirve como la ventana principal de la aplicación.

En la siguiente línea se crea el marco del widget, el cual en este caso contendrá la etiqueta y el botón que crearemos después. El marco encaja dentro de la ventana raíz.

La próxima línea crea una etiqueta para el widget que contiene una cadena de texto estática. El método grid() es usado para especificar la posición del diseño de la etiqueta que está dentro del marco del widget, similar a como trabajan las tablas en HTML.

Es entonces cuando el botón del widget es creado y dejado a la derecha de la etiqueta. Una vez pulsado llamará al método destroy() de la ventana raíz.

Finalmente el método mainloop() muestra todo en pantalla y responde a la entrada del usuario hasta que el programa termina.

Conceptos importantes de Tk
Incluso con este sencillo programa se pueden mostrar conceptos claves de Tk:

widgets
Una interfaz de usuario de Tkinter está compuesta de varios widgets individuales. Cada widget es representado como un objeto de Python, instanciado desde clases tales como ttk.Frame, ttk.Label y ttk.Button.

jerarquía de los widgets
Los widgets se organizan en jerarquías. La etiqueta y el botón estaban dentro del marco el que a su vez estaba contenido dentro del marco raíz. Cuando se crea cada child del widget su parent es pasado como el primer argumento del constructor del widget.

opciones de configuración
Los widgets tienen opciones de configuración los cuales modifican su apariencia y comportamiento, como el texto que se despliega en la etiqueta o el botón. Diferentes clases de widgets tendrán diferentes conjuntos de opciones.

gestión del diseño de formularios
Los widgets no son añadidos automáticamente a la interfaz de usuario una vez que han sido creados. Un administrador de geometrías como grid controla donde la interfaz de usuario es colocada.

ejecución del evento
Tkinter reacciona a la entrada del usuario, realiza los cambios del programa e incluso actualiza lo que se muestra en pantalla solo cuando se ejecuta activamente un event loop. Si el programa no esta ejecutando el evento, su interfaz de usuario no se actualizará.

Entendiendo como funcionan los empaquetadores de Tcl/Tk
When your application uses Tkinter’s classes and methods, internally Tkinter is assembling strings representing Tcl/Tk commands, and executing those commands in the Tcl interpreter attached to your application’s Tk instance.

Tanto si se está intentando navegar en la documentación de referencia o intentando encontrar el método u opción correcta o adaptando algún código ya existente o depurando su aplicación en Tkinter, hay veces que es útil entender como son esos comandos subyacentes de Tcl/Tk.

Para ilustrar, aquí se tiene un equivalente en Tcl/Tk de la parte principal del script Tkinter anterior.

ttk::frame .frm -padding 10
grid .frm
grid [ttk::label .frm.lbl -text "Hello World!"] -column 0 -row 0
grid [ttk::button .frm.btn -text "Quit" -command "destroy ."] -column 1 -row 0
La sintaxis es similar a varios lenguajes de línea de comandos, donde la primera palabra es el comando que debe ser ejecutado con los argumentos que le siguen al comando, separados por espacios, Sin entrar en demasiados detalles, note lo siguiente:

Los comandos usados para crear widgets (tales como ttk::frame) corresponden a las clases de widgets en Tkinter.

Opciones de widget en Tcl (por ejemplo -text) corresponden a argumentos clave en Tkinter.

Los widgets son reverenciados con un pathname in Tcl (por ejemplo .frm.btn) mientras que Tkinter no usa nombres sino que referencia a objetos.

El lugar del widget en la jerarquía es codificada (jerárquicamente) en nombre de archivo, usando un . (punto) como separador de búsqueda. El nombre de ruta del archivo para la ventana raíz es un . (punto), la jerarquía no es definida por la ruta del archivo pero si por la especificación del widget padre cuando se crea cada widget hijo.

Las operaciones que se implementan como commands separados en Tcl (por ejemplo grid or destroy) son representados como methods en objetos de widget de Tkinter. Como se verá en breve, en otras ocasiones, Tcl usa lo que parecen ser llamadas a métodos en objetos de widget, que reflejan más de cerca lo que se usa en Tkinter.

¿Cómo lo hago?, ¿Cómo funciona?
Si no esta seguro sobre como realizar ciertas acciones en Tkinter y no se puede encontrar información de forma rápida en el tutorial o en la documentación de referencias que estas usando, aquí hay varias estrategias que pueden ser de mucha ayuda.

Primero, recordar que los detalles sobre como cada widget trabaja en las diferentes versiones de Tcl/Tk pueden variar según la versión. Si está buscando información, asegúrese de que corresponde a la versión tanto de Python como de Tcl/Tk instaladas en su sistema.

Cuando se busca sobre como usar una API, puede ser muy útil el saber exactamente el nombre de la clase, opción o método que está utilizando. Tener esto en cuenta de antemano, ya sea en un shell de Python interactivo o con print(), puede ayudar a identificar lo que necesitas.

Para encontrar que opciones de configuración están disponibles en cada widget, se utiliza la llamada al método configure(), el cual retorna un diccionario que contiene una gran variedad de información sobre cada objeto, incluyendo valores por defecto y actuales. Utilice el keys() para saber los nombres de cada opción.

btn = ttk.Button(frm, ...)
print(btn.configure().keys())
La mayoría de los widgets tienen varias opciones de configuraciones en común, puede ser útil averiguar cuales son específicos de una clase de widget en particular. Comparando la lista de opciones de un widget sencillo como por ejemplo un frame es una de las formas de hacerlo.

print(set(btn.configure().keys()) - set(frm.configure().keys()))
De manera similar se pueden encontrar los métodos disponibles para un objeto de un widget utilizando la función standard dir(). Si lo intentas podrás ver alrededor de 200 métodos comunes para los widgets por lo que de nuevo, es muy importante identificar las especificaciones de un widget.

print(dir(btn))
print(set(dir(btn)) - set(dir(frm)))
Navegando en el manual de referencia de Tcl/Tk
Como se ha señalado, el manual oficial de referencia de Tk commands (en las páginas del manual) es a menudo la descripción mas precisa sobre que hacen las operaciones especificas de los widgets. Incluso sabiendo el nombre de la opción o método que necesitas, es posible que puedas encontrar información adicional.

Mientras todas las operaciones en Tkinter son implementadas como llamadas de método sobre objetos del widget, se puede ver cuantas operaciones de Tcl/Tk aparecen como comandos que toman el nombre de ruta del widget como el primer parámetro, seguido de parámetros opcionales, por ejemplo.

destroy .
grid .frm.btn -column 0 -row 0
Otros, sin embargo, se parecen más a métodos llamados en un objeto del widget (de hecho, cuando se crea un widget en Tcl / Tk, este a su vez crea un comando Tcl con el nombre de la ruta del widget, siendo el primer parámetro de ese comando el nombre de un método para llamar).

.frm.btn invoke
.frm.lbl configure -text "Goodbye"
En la documentación oficial de referencia para Tcl/Tk, se podrán encontrar mas operaciones que son similares a las llamadas a métodos de la pagina principal de un widget en especifico ( p.ej., se podrá encontrar el método invoke() en la página principal de ttk::button ) mientras que las funciones que toman un widget como un parámetro a menudo tienen su propia página principal (p.ej., grid).

Se podrán encontrar varias opciones en común y metidos en las páginas del manual options o ttk::widget, mientras que otras opciones se encuentran en las páginas del manual de una clase de widget específica.

También se puede encontrar cuantos métodos de Tkinter poseen nombres compuestos, p.ej, winfo_x(), winfo_height(), winfo_viewable(). Usted también encuentra información para todo esto en las páginas del manual en winfo <https://www.tcl.tk/man/tcl8.6/TkCmd/winfo.htm>`_.

Nota Puede ser algo confuso, pero también hay métodos en todos los widgets de Tkinter los cuales no operan directamente en el widget pero operan a nivel global, independiente del widget. Ejemplos de esto son los métodos para acceder al portapapeles o a llamados del sistema. (Ellos aparecen al ser implementados como métodos en la clase base Widget clase que todos los widgets en Tkinter heredan).
Modelo de subprocesamiento
Python y Tcl/Tk tienen modelos de subprocesamiento muy diferentes, con lo cual el módulo tkinter intenta ser un puente entre ambos. Si usted usa hilos, es muy posible que deba ser consciente de esto.

Un interprete de Python puede tener varios hilos asociados. En Tcl, diferentes hilos pueden ser creados, pero cada hilo tiene una instancia del interprete de Tcl asociada por separado. Los hilos también pueden crear mas de una instancia del interprete, aunque cada instancia del intérprete puede ser usada solo por el hilo que ha sido creado.

Cada objeto de la clase Tk creado por tkinter contiene un interprete para TCL. Esto también mantiene un seguimiento de cada hilo creado por el interprete. Las llamadas al módulo tkinter se pueden realizar desde cualquier módulo de Python. De forma interna si una llamada proviene desde un hilo diferente al creado por objeto Tk, un evento es publicado por la cola de eventos del interprete y cuando es ejecutado, el resultado es retornado al hilo de llamadas de Python.

Las aplicaciones de Tcl/Tk son normalmente basados en eventos, lo que significa que después de la inicialización, el interprete corre un bucle de eventos (p.ej. Tk.mainloop()) que responde a los eventos. Al ser un evento de un solo proceso, los controladores responden rápidamente, en caso contrario ellos bloquearán el procesamiento de otros eventos. Para evitar esto, cualquier procesamiento de larga duración no deben ser procesados en el controlador de eventos, o en caso contrario ser separados en pequeñas piezas usando temporizadores o correr el proceso en otro hilo. Esto es diferente a muchos kits de herramientas de GUI donde GUI se ejecuta en un hilo completamente separado del código de la aplicación, incluido el controlador de eventos.

Si el intérprete de Tcl no se está ejecutando el bucle del evento o procesando eventos, cualquier llamada a tkinter realizada desde hilos distintos al que ejecuta el intérprete de Tcl, fallará.

Existen varios casos especiales:

Las librerías de Tcl/Tk pueden ser construidas para que no sean conscientes de los subprocesos. En este caso, el tkinter llama a la librería a partir del hilo originado desde Python, incluso si este es diferente al hilo creado en el intérprete de Tcl. Un cierre global asegura que solo ocurra una llamada a la vez.

Mientras el tkinter permite crear una o mas instancias del objeto de Tk (con su propio intérprete), todos los interpretes son parte del mismo hilo compartido en la cola de eventos, lo cual se torna complicado rápidamente. En la practica, no se debe crear mas de una instancia de la clase Tk a la vez. De lo contrario, es lo mejor crearlos en hilos separados y asegurarse de que se esta ejecutando un hilo compatible con los subprocesos.

Bloquear los controladores de eventos no son la única manera de prevenir que el intérprete de Tcl reingrese al bucle de eventos. Es incluso posible ejecutar una cantidad de bucles de eventos anidados o abandonar el bucle de forma completa, Si está realizando alguna tarea complicada cuando se trata de eventos o hilos hay que tener en cuenta estas posibilidades.

Hay algunas funciones select tkinter que ahora solo funcionan cuando se les llama desde el hilo que creó el intérprete Tcl.

Guía práctica
Configuración de opciones
Las opciones controlan parámetros como el color y el ancho del borde de un widget. Las opciones se pueden configurar de tres maneras:

En el momento de la creación del objeto, utilizando argumentos de palabras clave
fred = Button(self, fg="red", bg="blue")
Después de la creación del objeto, tratando el nombre de la opción como un índice de diccionario
fred["fg"] = "red"
fred["bg"] = "blue"
Usando el método config() para actualizar múltiples atributos después de la creación del objeto
fred.config(fg="red", bg="blue")
Para obtener una explicación completa de las opciones y su comportamiento, consulte las páginas de manual de Tk para el widget en cuestión.

Tenga en cuenta que las páginas del manual enumeran «OPCIONES ESTÁNDAR» y «OPCIONES ESPECÍFICAS DE WIDGET» para cada widget. La primera es una lista de opciones que son comunes a muchos widgets, la segunda son las opciones que son específicas para ese widget en particular. Las opciones estándar están documentadas en la página del manual options(3).

No se hace distinción entre las opciones estándar y las opciones específicas del widget en este documento. Algunas opciones no se aplican a algunos tipos de widgets. Si un determinado widget responde a una opción particular depende de la clase del widget. Los botones tienen la opción command, las etiquetas no.

Las opciones admitidas por un widget dado se enumeran en la página de manual de ese widget, o se pueden consultar en tiempo de ejecución llamando al método config() sin argumentos, o llamando al método keys() en ese widget. El valor retornado en esas llamadas es un diccionario cuya clave es el nombre de la opción como una cadena (por ejemplo, 'relief') y cuyo valor es una tupla de 5 elementos.

Algunas opciones, como bg, son sinónimos de opciones comunes con nombres largos (bg es la abreviatura de «background»). Al pasar el método config(), el nombre de una opción abreviada retornará una tupla de 2 elementos (en lugar de 5). Esta tupla contiene nombres de sinónimos y nombres de opciones «reales» (como ('bg', 'background')).

Índice

Significado

Ejemplo

0

nombre de la opción

'relief'

1

nombre de la opción para la búsqueda en la base de datos

'relief'

2

clase de la opción para la consulta de base de datos

'Relief'

3

valor por defecto

'raised'

4

valor actual

'groove'

Ejemplo:

>>>
print(fred.config())
{'relief': ('relief', 'relief', 'Relief', 'raised', 'groove')}
Por supuesto, el diccionario impreso incluirá todas las opciones disponibles y sus valores. Esto es solo un ejemplo.

El empaquetador
El empaquetador es uno de los mecanismos de gestión de geometría de Tk. Los administradores de geometría se utilizan para especificar la posición relativa de los widgets dentro de su contenedor: su master mutuo. En contraste con el placer más engorroso (que se usa con menos frecuencia, y no cubrimos aquí), el empaquetador toma la especificación de relación cualitativa - above, to the left of, filling, etc - y funciona todo para determinar las coordenadas de ubicación exactas para usted.

El tamaño de cualquier widget maestro está determinado por el tamaño del widget esclavo interno. El empaquetador se usa para controlar dónde se colocará el widget esclavo en el widget maestro de destino. Para lograr el diseño deseado, puede empaquetar el widget en un marco y luego empaquetar ese marco en otro. Además, una vez empaquetado, la disposición se ajusta dinámicamente de acuerdo con los cambios de configuración posteriores.

Tenga en cuenta que los widgets no aparecen hasta que su geometría no se haya especificado con un administrador de diseño de pantalla. Es un error común de principiante ignorar la especificación de la geometría, y luego sorprenderse cuando se crea el widget pero no aparece nada. Un objeto gráfico solo aparece después que, por ejemplo, se le haya aplicado el método pack() del empaquetador.

Se puede llamar al método pack() con pares palabra-clave/valor que controlan dónde debe aparecer el widget dentro de su contenedor y cómo se comportará cuando se cambie el tamaño de la ventana principal de la aplicación. Aquí hay unos ejemplos:

fred.pack()                     # defaults to side = "top"
fred.pack(side="left")
fred.pack(expand=1)
Opciones del empaquetador
Para obtener más información sobre el empaquetador y las opciones que puede tomar, consulte el manual y la página 183 del libro de John Ousterhout.

anchor
Tipo de anclaje. Indica dónde debe colocar el empaquetador a cada esclavo en su espacio.

expand
Un valor booleano, 0 o 1.

fill
Los valores legales son: 'x', 'y', 'both', 'none'.

ipadx y ipady
Una distancia que designa el relleno interno a cada lado del widget esclavo.

padx y pady
Una distancia que designa el relleno externo a cada lado del widget esclavo.

side
Los valores legales son: 'left', 'right', 'top', 'bottom'.

Asociación de variables de widget
La asignación de un valor a ciertos objetos gráficos (como los widgets de entrada de texto) se puede vincular directamente a variables en su aplicación utilizando opciones especiales. Estas opciones son variable, textvariable, onvalue, offvalue, y value. Esta conexión funciona en ambos sentidos: si la variable cambia por algún motivo, el widget al que está conectado se actualizará para reflejar el nuevo valor.

Desafortunadamente, en la implementación actual de tkinter no es posible entregar una variable arbitraria de Python a un widget a través de una opción variable o textvariable . Los únicos tipos de variables para las cuales esto funciona son variables que son subclases de la clase Variable, definida en tkinter.

Hay muchas subclases útiles de Variable ya definidas: StringVar, IntVar, DoubleVar, and BooleanVar. Para leer el valor actual de dicha variable, es necesario llamar al método get(), y para cambiar su valor, al método set(). Si se sigue este protocolo, el widget siempre rastreará el valor de la variable, sin ser necesaria ninguna otra intervención.

Por ejemplo:

import tkinter as tk

class App(tk.Frame):
    def __init__(self, master):
        super().__init__(master)
        self.pack()

        self.entrythingy = tk.Entry()
        self.entrythingy.pack()

        # Create the application variable.
        self.contents = tk.StringVar()
        # Set it to some value.
        self.contents.set("this is a variable")
        # Tell the entry widget to watch this variable.
        self.entrythingy["textvariable"] = self.contents

        # Define a callback for when the user hits return.
        # It prints the current value of the variable.
        self.entrythingy.bind('<Key-Return>',
                             self.print_contents)

    def print_contents(self, event):
        print("Hi. The current entry content is:",
              self.contents.get())

root = tk.Tk()
myapp = App(root)
myapp.mainloop()
El gestor de ventanas
En Tk hay un comando útil, wm, para interactuar con el gestor de ventanas. Las opciones del comando wm le permiten controlar cosas como títulos, ubicación, iconos de ventana y similares. En tkinter, estos comandos se han implementado como métodos de la clase Wm. Los widgets de alto nivel son subclases de Wm, por lo que se puede llamar directamente a los métodos de Wm.

Para acceder a la ventana Toplevel que contiene un objeto gráfico dado, a menudo puede simplemente referirse al padre de este objeto gráfico. Por supuesto, si el objeto gráfico fue empaquetado dentro de un marco, el padre no representará la ventana de nivel superior. Para acceder a la ventana de nivel superior que contiene un objeto gráfico arbitrario, puede llamar al método _root() . Este método comienza con un subrayado para indicar que esta función es parte de la implementación y no de una interfaz a la funcionalidad Tk.

Aquí hay algunos ejemplos típicos:

import tkinter as tk

class App(tk.Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.pack()

# create the application
myapp = App()

#
# here are method calls to the window manager class
#
myapp.master.title("My Do-Nothing Application")
myapp.master.maxsize(1000, 400)

# start the program
myapp.mainloop()
Tipos de datos de opciones Tk
anchor
Los valores legales son los puntos de la brújula: "n", "ne", "e", "se", "s", "sw", "w", "nw", y también "center".

bitmap
Hay ocho nombres de bitmaps integrados: 'error', 'gray25', 'gray50', 'hourglass', 'info', 'questhead', 'question', 'warning'. Para especificar un nombre de archivo de mapa de bits de X, indique la ruta completa del archivo, precedida por una @, como en "@/usr/contrib/bitmap/gumby.bit".

boolean
Se puede pasar enteros 0 o 1 o las cadenas "yes" or "no".

callback
Esto es cualquier función de Python que no toma argumentos. Por ejemplo:

def print_it():
    print("hi there")
fred["command"] = print_it
color
Los colores se pueden dar como nombres de colores de X en el archivo rgb.txt, o como cadenas que representan valores RGB. La cadena que representa el valor RGB toma un rango de 4 bits:"#RGB", 8 bits: "#RRGGBB", 12 bits» "#RRRGGGBBB", o 16 bits "#RRRRGGGGBBBB" donde R, G, B representan cualquier dígito hexadecimal legal. Consulte la página 160 del libro de Ousterhout para más detalles.

cursor
Los nombres estándar del cursor X de cursorfont.h se pueden usar sin el prefijo XC_. Por ejemplo, para obtener un cursor de mano (XC_hand2), use la cadena "hand2". También se puede especificar su propio mapa de bits y archivo de máscara. Ver página 179 del libro de Ousterhout.

distance
Las distancias de pantalla se pueden especificar tanto en píxeles como en distancias absolutas. Los píxeles se dan como números y las distancias absolutas como cadenas con el carácter final que indica unidades: c para centímetros, i para pulgadas, m para milímetros, p para puntos de impresora. Por ejemplo, 3.5 pulgadas se expresa como "3.5i".

font
Tk usa un formato de lista para los nombres de fuentes, como {courier 10 bold}. Los tamaños de fuente expresados en números positivos se miden en puntos, mientras que los tamaños con números negativos se miden en píxeles.

geometry
Es una cadena de caracteres del estilo widthxheight, donde el ancho y la altura se miden en píxeles para la mayoría de los widgets (en caracteres para widgets que muestran texto). Por ejemplo: fred["geometry"] = "200x100".

justify
Los valores legales son las cadenas de caracteres: "left", "center", "right", y "fill".

region
Es una cadena de caracteres con cuatro elementos delimitados por espacios, cada uno de ellos es una distancia legal (ver arriba). Por ejemplo: "2 3 4 5" , "3i 2i 4.5i 2i" y "3c 2c 4c 10.43c" son todas regiones legales.

relief
Determina cuál será el estilo de borde de un widget. Los valores legales son: "raised", "sunken", "flat", "groove", y "ridge".

scrollcommand
Este es casi siempre el método set() de algún widget de barra de desplazamiento, pero puede ser cualquier método de un widget que tome un solo argumento.

wrap
Debe ser uno de estos: "none", "char", o "word".

Enlaces y eventos
El método de enlace (binding) del comando del widget le permite observar ciertos eventos y hacer que la función de devolución de llamada se active cuando se produce ese tipo de evento. La forma del método de enlace es:

def bind(self, sequence, func, add=''):
donde:

sequence
es una cadena que denota el tipo de evento objetivo. Para obtener más información, consulte la página del manual de Bind y la página 201 del libro de John Ousterhout.

func
es una función de Python que toma un argumento y se llama cuando ocurre el evento. La instancia del evento se pasa como el argumento mencionado. (Las funciones implementadas de esta manera a menudo se llaman callbacks.)

add
es opcional, ya sea '' o '+'. Pasar una cadena de caracteres vacía indica que este enlace anulará cualquier otro enlace asociado con este evento. Pasar '+' agrega esta función a la lista de funciones vinculadas a este tipo de evento.

Por ejemplo:

def turn_red(self, event):
    event.widget["activeforeground"] = "red"

self.button.bind("<Enter>", self.turn_red)
Observe cómo se accede al campo del widget del evento en la devolución de llamada turn_red(). Este campo contiene el widget que capturó el evento de X. La siguiente tabla enumera los otros campos de eventos a los que puede acceder y cómo se indican en Tk, lo que puede ser útil cuando se hace referencia a las páginas del manual de Tk.

Tk

Campo evento de Tkinter

Tk

Campo evento de Tkinter

%f

focus

%A

char

%h

height

%E

send_event

%k

keycode

%K

keysym

%s

state

%N

keysym_num

%t

time

%T

type

%w

width

%W

widget

%x

x

%X

x_root

%y

y

%Y

y_root

El parámetro índice
Muchos widgets requieren que se pase un parámetro de tipo índice. Esto se utiliza para señalar ubicaciones específicas en el widget de texto, caracteres específicos en el widget de entrada, o elementos particulares en el widget de menú.

Índice de widget de entrada (índice, índice de vista, etc.)
El widget entrada tiene una opción para referirse a la posición de los caracteres del texto que se muestra. Puede acceder a estos puntos especiales en un widget de texto utilizando la siguiente función de tkinter:

Índice de widget de texto
La notación de índice del widget de texto es muy rica y se detalla mejor en las páginas del manual de Tk.

Índices de menú (menu.invoke(), menu.entryconfig(), etc.)
Algunas opciones y métodos para menús manipulan entradas de menú específicas. Cada vez que se necesita un índice de menú para una opción o un parámetro, se puede pasar:

un número entero que se refiere a la posición numérica de la entrada en el widget, contada desde arriba, comenzando con 0;

la cadena "active", que se refiere a la posición del menú que está actualmente debajo del cursor;

la cadena de caracteres "last" que se refiere al último elemento del menú;

Un número entero precedido por @, como en @6, donde el entero es interpretado como una coordenada y de píxeles en el sistema de coordenadas del menú;

la cadena de caracteres "none", que indica que no hay entrada de menú, usado frecuentemente con menu.activate() para desactivar todas las entradas; y, finalmente,

una cadena de texto cuyo patrón coincide con la etiqueta de la entrada del menú, tal como se explora desde la parte superior del menú hasta la parte inferior. Tenga en cuenta que este tipo de índice se considera después de todos los demás, lo que significa que las coincidencias para los elementos del menú etiquetados last, active, o none pueden interpretarse de hecho como los literales anteriores.

Imágenes
Se pueden crear imágenes de diferentes formatos a través de la correspondiente subclase de tkinter.Image:

BitmapImage para imágenes en formato XBM.

PhotoImage para imágenes en formatos PGM, PPM, GIF y PNG. Este último es compatible a partir de Tk 8.6.

Cualquier tipo de imagen se crea a través de la opción file o data (también hay otras opciones disponibles).

El objeto imagen se puede usar siempre que algún widget admita una opción de image (por ejemplo, etiquetas, botones, menús). En estos casos, Tk no mantendrá una referencia a la imagen. Cuando se elimina la última referencia de Python al objeto de imagen, los datos de la imagen también se eliminan, y Tk mostrará un cuadro vacío donde se utilizó la imagen.

Ver también El paquete Pillow añade soporte para los formatos del tipo BMP, JPEG, TIFF, y WebP, entre otros.
Gestor de archivos
Tk permite registrar y anular el registro de una función de devolución de llamada que se llamará desde el mainloop de Tk cuando la E/S sea posible en un descriptor de archivo. Solo se puede registrar un controlador por descriptor de archivo. Código de ejemplo:

import tkinter
widget = tkinter.Tk()
mask = tkinter.READABLE | tkinter.WRITABLE
widget.tk.createfilehandler(file, mask, callback)
...
widget.tk.deletefilehandler(file)
Esta función no está disponible en Windows.

Dado que no se sabe cuántos bytes están disponibles para su lectura, no use métodos de BufferedIOBase o TextIOBase read() o readline(), ya que estos requieren leer un número predefinido de bytes. Para sockets, los métodos recv() o recvfrom() trabajan bien; para otros archivos, use lectura sin formato o os.read(file.fileno(), maxbytecount).

Widget.tk.createfilehandler(file, mask, func)
Registra la función callback gestor de archivos func. El argumento file puede ser un objeto con un método fileno() (como un archivo u objeto de socket), o un descriptor de archivo entero. El argumento mask es una combinación ORed de cualquiera de las tres constantes que siguen. La retrollamada se llama de la siguiente manera:

callback(file, mask)
Widget.tk.deletefilehandler(file)
Anula el registro de un gestor de archivos.

tkinter.READABLE
tkinter.WRITABLE
tkinter.EXCEPTION
Constantes utilizadas en los argumentos mask.