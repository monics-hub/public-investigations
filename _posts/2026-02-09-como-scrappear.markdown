---
layout: post
title: ¿Cómo realizar un flujo de web scrapping en n8n?
date: 2026-02-09
description: Some post about ¿Cómo conversan los peruanos sobre los candidatos a la presidencia?
img: https://raw.githubusercontent.com/monics-hub/public-investigations/refs/heads/master/assets/img/flow.png
---
El día de hoy veremos como se realiza web scrapping a un blog desde un flujo de n8n.

1. Seleccionar una url a Scrappear.
En este caso tomaremos como ejemplo la siguiente: https://www.dinamic.site/investigacionespublicas. Podemos observar que dicho sitio web contiene una lista de posts sobre investigaciones públicas que el creador del sitio web (Dinamics) a realizado. Sin embargo es de notar que únicamente colocaron un título, foto, fecha y un documento PDF en cada post. Sin embargo dichos posts carecen de contenido textual, y el contenido es visible fácilmente. El objetivo será entonces, utilizar Web Scrapping para extraer el contenido de dichos artículos para mejorarlos a través de agregar una descripción y pasar los pdf a imagenes.

2. Crear una instancia de EC2.
Procedemos a crear una instancia de n8n, la cual podemos utilizar en local o hacerlo mediante la nube. En mi caso utilizaré una instancia EC2 de AWS la cual me permite tener el flujo de trabajo de n8n corriendo diariamente en la nube para en automático, ejecutar un script casa cierto tiempo para extraer nuevos posts en caso de que se publiquen. Para lo cual después de haber creado una cuenta de AWS Cloud procedemos a crear una instancia de EC2 base linux. 

3. Instalar Docker.
Una vez tengamos nuestra distribuación de linux lista procedemos a ejecutar una serie de comandos en terminal para instalar Docker, Docker Compose y para descargar la imagen del contenedor de n8n disponible en Dockerhub.

4. Crear un flujo de n8n.
Una vez listo el servidor de n8n, empezamos añadiendo un primer nodo para ejecutar el workflow al dar clic como trigger.

5. Nodo HTTP request.
En la sección de nodos "core" podemos hallar un nodo para realizar una request HTTP. En la configuración seleccionamos la URL a Scrappear.

6. Nodo Scrapper.
El nodo más importante. Es un nodo de tipo "código", que para este ejemplo se selecciono JavaScript como lenguaje ya que a diferencia de Python este no require la instalación de bibliotecas externas para realizar web scrapping (a diferencia de Python que requiere Beautiful Soup). Ya que JavaScript provee herramientas nativas para trabajar con documentos HTML.

Este nodo es la parte dificil y su código variará dependiendo del sitio web a Scrappear. En este caso tenemos que trabajar sobre un documento HTML generado con Wix, el cual suele generar código bastante repetitivo y criptico. Recomiendo descargar el response del nodo anterior y abrirlo en un editor de texto, y buscar palabras claves de lo que lo queremos Scrappear.

Cómo ejemplo si queremos extraer el título empezamos por extraer la información manualmente de los primeros dos ejemplos:

 * ¿Cómo conversan los peruanos sobre los candidatos a la presidencia?
 * EC | Coche bomba en Guayaquil - el ataque que reavivó la crisis de seguridad en Ecuador

Posteriormente buscamos dichos titulos en la respuesta que obtuvimos (usando una herramienta de busqueda de un editor de texto). Una vez encontradas dichas instancias (que en este caso son muchas ya que Wix repite la información), tenemos dos opciones. Crear las cadenas de regex manualmente o copiar un fragmento de la respuesta y darsela a un LLM para que genere el codigo Regex que me de la información que necesito. Para este caso el código generado fue el siguiente.

7. Sobrepasar la paginación por defecto de Wix.

8. Resumir el texto con un nodo LLM.

9. Nodos para dar formato.

10. Publicar el contenido extraido en Jekyll.