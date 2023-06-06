# roulette
Este proyecto es una aplicación de ruleta desarrollada con Vue 3 y la librería de componentes vue3-roulette. El propósito de la aplicación es permitir a los usuarios "girar la ruleta" para ganar premios variados.

Características
El código implementa las siguientes características:

Una ruleta gráfica que se puede girar.
Los ítems en la ruleta están predefinidos en el código y pueden ser cambiados a su gusto.
Cada ítem tiene un peso asociado que determina la probabilidad de que sea seleccionado.
Los ítems pueden ser habilitados o deshabilitados, permitiendo cierta flexibilidad en la configuración.
Se muestra un popup cada vez que la ruleta se detiene en un ítem, permitiendo al usuario "aceptar" o "rechazar" el premio.
Se lleva un seguimiento de la cantidad de premios ganados por el usuario y se limita la cantidad de ciertos premios que se pueden ganar.
Librerías Utilizadas
Este proyecto utiliza las siguientes librerías:

vue3-roulette: Esta es la librería principal utilizada para implementar la ruleta. Proporciona un componente de ruleta personalizable que se puede integrar fácilmente en cualquier aplicación Vue.
vue-confetti-explosion: Esta librería se utiliza para proporcionar una animación de confeti que se despliega cuando el usuario gana un premio.
Cómo usar
Para utilizar este código en tu propio proyecto:

Clona o descarga este repositorio.
Instala las dependencias del proyecto con npm install.
Corre el proyecto con npm run serve.
Personaliza los ítems de la ruleta, los pesos, los límites de los premios y cualquier otra característica según sea necesario.
Próximas Mejoras
Algunas ideas para futuras mejoras incluyen:

Permitir la personalización de la ruleta y los premios a través de una interfaz gráfica en lugar de tener que modificar el código.
Añadir la capacidad de guardar el estado del juego en una base de datos para permitir a los usuarios continuar donde lo dejaron.
Implementar un sistema de autenticación para permitir que múltiples usuarios jueguen en la misma aplicación.
Contribuciones
Las contribuciones son bienvenidas. Por favor, abre una issue o realiza un pull request si tienes alguna mejora que te gustaría implementar o sugerir.
