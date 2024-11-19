# Practica_10

## Integrantes:
  •	Luis Fernando Duarte Reséndiz
  
  •	Mauricio Alberto Gómez Arroyo
  
  •	Diego Brandon Guzmán Sierra
  
  •	Bryan Hiadim Vera Hernández
  
## Introducción:
En esta práctica se busca implementar un proceso automatizado que permita a un robot Epson reproducir un dibujo a partir de una imagen digital. Para lograrlo, se utiliza el software MATLAB como herramienta principal para procesar la imagen, transformándola en una matriz binaria que sirva como guía para los movimientos del robot.

El procedimiento incluye varios pasos clave: desde la configuración inicial en MATLAB, donde se convierte la imagen en un formato adecuado, hasta la creación de un archivo de texto con las coordenadas que el robot interpretará para realizar el dibujo. Posteriormente, estas coordenadas son cargadas y ejecutadas por el robot mediante un programa específico que controla su funcionamiento.

Este proyecto combina conocimientos de procesamiento de imágenes, programación, y control robótico, ofreciendo una aplicación práctica en la interacción entre software y hardware para tareas de automatización. Al finalizar, el robot será capaz de replicar con precisión el dibujo, siguiendo el diseño binario generado a partir de la imagen inicial.

## Instrucciones:
Para esta práctica, se requiere el uso del software MATLAB y una imagen en formato JPEG con una resolución pequeña, preferiblemente de 100x100 píxeles.

![image](https://github.com/user-attachments/assets/1594f6ec-c9a5-4ffd-ad4c-e01e3b420d4d)

### Configuración en MATLAB:

1.	Abrir MATLAB y crear un nuevo proyecto:

Una vez creado, se debe guardar la imagen dentro de la carpeta correspondiente al proyecto para que pueda ser leída por el programa.

![image](https://github.com/user-attachments/assets/d7f4547d-d8f0-49e4-a1f7-a2c5bf16cc33)

2.	Escribir el siguiente código en el editor de MATLAB:

```plaintext
clear all
clc
color = imread ("foto.jpg");
imshow(color)
 
gris=rgb2gray(color);
figure
imshow(gris)
 
blancoynegro = imbinarize(gris, 'adaptive', 'sensitivity', 0.7)
figure
blancoynegro=not(blancoynegro)
imshow(not(blancoynegro))
 
blancoynegro=sparse(blancoynegro)
```

Explicando un poco el código tenemos que:

  •	imread carga la imagen en color.
  
  •	rgb2gray convierte la imagen a escala de grises.
  
  •	imbinarize transforma la imagen en un formato binario, donde los píxeles son 0 (blanco) o 1 (negro).
  
  •	sparse genera una matriz dispersa que contiene únicamente los valores binarios relevantes para optimizar el uso de memoria.
  
El objetivo de este código es procesar la imagen para que pueda ser interpretada por un robot Epson, donde los valores 1 y 0 se traducen como "pintar" y "no pintar", respectivamente.

una vez se tiene este código obtenemos lo siguiente:



![image](https://github.com/user-attachments/assets/ae42b4ee-fe92-43f2-9016-d7b5250a06e7)

    Foto original.


![image](https://github.com/user-attachments/assets/a779d8f9-9d90-45ff-8b53-7ce5ea633a47)

    Foto a blanco y negro.


![image](https://github.com/user-attachments/assets/116a5bfa-b916-4ba3-95d0-773793300693)

    Foto final.



Esto básicamente lo que hace es generar 0 y 1 que el robot Epson debe interpretar como pintar y no pintar, de esta manera hará el dibujo que se le pida.

### Obtención y formato de los datos.

Después de ejecutar el código, se deben recopilar los valores generados en la ventana de comandos (Command Window). Estos valores representan las coordenadas en las que el robot debe pintar.

1.	Copiar los datos obtenidos y pegarlos en un archivo de texto.

![image](https://github.com/user-attachments/assets/d67f1522-27a0-4f23-882e-a7d1d3d179e8)

2.	Editar el archivo eliminando paréntesis, comas, valores de 1, y cualquier espacio innecesario. El formato final obtenido es el siguiente:

![image](https://github.com/user-attachments/assets/951f182e-2db2-49e7-a165-ade667b7529b)

Guardar este archivo de texto en la carpeta correspondiente al proyecto del robot Epson.

![image](https://github.com/user-attachments/assets/60960f7d-8d92-4072-9d56-76b42d22b672)

### Configuración del robot Epson
Una vez preparados los datos, se utiliza el siguiente código para cargar y ejecutar el dibujo:

```plaintext
Function main
Reset
Motor On
Power High

Speed 30
Accel 30, 30
SpeedS 600
AccelS 6000, 6000

String linea$, h$, yt$
Integer x, y, largo, archivo, espacio, i
Boolean fin

fin = 1
i = 1

archivo = FreeFile
ROpen "datos.txt" As #archivo

Do While fin
	Input #archivo, linea$
	largo = Len(linea$)
	espacio = InStr(linea$, " ")
	yt$ = Mid$(linea$, 1, espacio - 1)
	h$ = Mid$(linea$, espacio + 1, largo - espacio)
	x = Val(h$)
	y = Val(yt$)
	
	Print "columna= ", x, "renglon= ", y
	Go inicio +X(x / 1) -Y(y / 1)
	i = i + 1
	Move Here -Z(20)
	Move Here +Z(20)
	
	fin = Eof(archivo) + 1
	
	
Loop
Close #archivo

Fend

```

Explicando un poco el código tenemos:

•	Configuración inicial: Se inicializan los motores del robot, se ajusta la velocidad, y se definen las variables necesarias.

•	Carga del archivo: El robot lee el archivo datos.txt línea por línea, extrayendo las coordenadas X y Y.

•	Movimiento del robot:

o	El robot se posiciona en las coordenadas especificadas.

o	Realiza un movimiento para "pintar" (baja el cabezal) y luego regresa a su posición inicial.

•	Cierre del archivo: Una vez procesadas todas las coordenadas, el archivo se cierra y el programa termina.



 ### Ejecución del dibujo.
 
Con todos los pasos completados, el robot Epson estará listo para realizar el dibujo procesado a partir de la imagen. El resultado final será una representación en blanco y negro según las coordenadas proporcionadas.

![image](https://github.com/user-attachments/assets/48c2af80-47ac-4da0-bb3e-f54d50a1f4bb)


## Conclusiones  

Luis Fernando Duarte Reséndiz:

El desarrollo de esta práctica permitió comprender la importancia del procesamiento de imágenes como herramienta para automatización en robótica. El uso de MATLAB resultó fundamental para transformar datos visuales en información interpretable para el robot Epson, destacando la relevancia del trabajo en equipo y la aplicación práctica de conocimientos adquiridos previamente.  

Mauricio Alberto Gómez Arroyo:

A través de este proyecto, se demostró cómo la integración entre software y hardware puede simplificar procesos complejos como la reproducción de imágenes por un robot. Además, la práctica reforzó habilidades en programación y análisis de datos, mostrando la versatilidad de MATLAB y los sistemas robóticos en aplicaciones reales.  

Diego Brandon Guzmán Sierra:

La experiencia adquirida en esta actividad resaltó el valor de la planificación y la organización en proyectos multidisciplinarios. El entendimiento de cómo convertir una imagen en un patrón binario para su interpretación por un robot fue clave para consolidar los conceptos de automatización y control.  

Bryan Hiadim Vera Hernández:

Este proyecto permitió aplicar conocimientos en mecatrónica, especialmente en el diseño y ejecución de sistemas integrados. El uso del robot Epson evidenció cómo las herramientas tecnológicas actuales pueden ser utilizadas para resolver problemas prácticos, fortaleciendo habilidades técnicas y colaborativas dentro del equipo.  







