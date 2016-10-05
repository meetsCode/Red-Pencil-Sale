Intento resolver el reto propuesto en:

https://github.com/ardalis/kata-catalog/blob/master/katas/Red%20Pencil%20Sale.md

en la kata:
https://www.meetup.com/es-ES/Barcelona-Software-Craftsmanship/events/233824572/

La intención del ejercicio es no solo implementar la exigencia del cliente en el paradigma de VCM de un lenguaje POO como es Smalltalk sino también sufrir los continuos cambios en los requerimientos que pide el cliente. Para ello no implementaremos cada uno de los 6 puntos del apartado <<Instructions>> de golpe sino uno a uno ignorando los siguientes. Para poder mostrar la evolución, tras cubrir cada punto guardaremos la versión en Gitup o en Monticelo. 

Para este ejercicio usaré una imagen Pharo5.0

La información del apartado <<Backgroud>> representa una típica exigencia de un cliente o producto owner. Como en cualquier conversación con el cliente, el modelo, el visor y controlador están entremezclados. La primera tarea será trasladar ese texto a un esquema.

Lo primero fue obtener los substantivos del Background:
Customer, Portal, Product, Promotion, Price y Rule y meterlos en una estructura de árbol. En el fichero <<Red Pencil sale esquema 01.png>> puede verse el esquema inicial.

En el primer punto:
A red pencil promotion starts due to a price reduction. However, the red pencil promotion is only applied if the price was reduced by at least 5% and at most 30%. Also, the previous price must have been stable (unchanged) for at least 30 days.

Aparece el concepto de promoción. La promoción tiene control sobre el objeto al que modifica. Eso significa que está por encima en el esquema de árbol. A su vez se necesita un histórico de los cambios que se llevan a cabo. Este asunto lo he resuelto a través de la clase ChangePrice. Esta recibe, por parte de un usuario externo un nuevo precio (o petición de aumento o disminución de porcentaje) y el producto o bien a cambiar. Este cambio de precio lo lleva a cabo pero el ChangePrice. Para el visor es importante conocer cuanto ha cambiado el precio respecto al precio anterior y cuando. Solo así el visor conoce si estamos ante una RedPencilPromotion o no. Por eso ChangePrice anota en sí mismo la fecha del cambio y el precio que está substituyendo. Una vez hecho el cambio ChangePrice se autoincluye en el histórico del bien.
Aquí dejo claro que decidir si es un RedPencil o no no depende del objeto “Good” ni tampoco del ChangePrice él solo conserva su histórico. Si es un RedPencil o cualquier otro tipo de oferta solo le interesa al visor que, en función de eso, mostrará una información u otra. 
A pesar de lo dicho y para no construir la interfaz en esta primera versión el método isRedPencil responderá true si el precio actual lo es.

