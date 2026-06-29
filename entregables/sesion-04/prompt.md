Actúas como Product Owner senior con amplia experiencia descomponiendo PRDs de productos SaaS en backlogs con user stories AI friendly.
Analiza el [PRD.md](http://PRD.md) de FlowSync. Lee la sección del MVP, analiza el producto, su alcance y genera user stories listas para el equipo de desarrollo + Agentes de IA.
#Non-goals:

* SOLO funcionalidades que estén en la sección de MVP del PRD.
* NO inventes features, pantallas ni integraciones que no aparezcan en el documento.
* NO estimes tiempos, esfuerzo ni story points.
* NO propongas arquitectura, stack técnico ni diseño de base de datos.
* Si algo NO está literal en el PRD pero necesitas inferirlo para que la story sea coherente, márcalo con "(asumido)" justo en esa parte.
#Formato

1. Cada story debe seguir EXACTAMENTE esta plantilla: "Como [rol], quiero [acción], para [beneficio]."
2. Agrupa las stories por módulo / épica / caso de uso, según lo que tenga más sentido para FlowSync. Usa un encabezado por grupo.
3. Cada story lleva un bloque de 3 a 5 criterios de aceptación en formato Given/When/Then.
Ejemplo (Replicar esta estructura para las stories):
Épica: Autenticación
Historia: Como usuario nuevo, quiero registrarme con mi correo y contraseña, para entrar a mi cuenta de FlowSync.

* GIVEN estoy en la pantalla de registro, WHEN ingreso un correo válido y una contraseña que cumple los requisitos, THEN mi cuenta es creada y puedo acceder al dashboard.
* GIVEN ingreso un correo ya registrado, WHEN intento crear la cuenta, THEN veo el mensaje "este correo ya está en uso" y no crea una cuenta repetida.
* GIVEN dejo el campo de correo vacío, WHEN pulso el botón "Registrarme", THEN el botón permanece deshabilitado y veo el mensaje "Completa el campo correo para continuar".
