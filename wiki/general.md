---
layout: article
title: Definiciones generales
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Base64</div>
</div>
Es un mecanismo de encodeado que originalmente se hizo para codificar datos en binario en formato de texto. Primero se uso en sistemas de correo que requerían adjuntar binarios como imágenes y documentos ‘rich-text’ para ser enviados en formato ASCII.
Es comúnmente utilizado por aplicaciones web para ocultar cosas como valores de parámetros, sesiones, etc. Bad choice…

El encodeado en base64 está compuesto por 64 caracteres ASCII.     [a-z] [A-Z] + / =

Para decodificar una cadena en base64 se pueden utilizar muchas formas, entre las más rápidas, o más "a mano" están ejecutar el siguiente comando en la terminal:

~~~bash
echo "<código en base64>" | base64 -d
echo "ZmxhZ3tQcm9EZWxCYXNlNjRfOCl9" | base64 -d
~~~

O también puedes utilizar esta web:
[https://www.base64decode.org/](https://www.base64decode.org/)
