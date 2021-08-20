# Medir-temperatura-Raspberry


Es importante destacar que los componentes que más sufren por temperatura en una Raspberry Pi son el procesador (CPU), y el controlador gráfico (GPU). Para conocer su temperatura, basta con usar dos comandos que se pueden ejecutar de forma nativa en el Sistema Operativo de la Raspberry:

Temperatura de la CPU: cat /sys/class/thermal/thermal_zone0/temp
Temperatura de la GPU: vcgencmd measure_temp

Si ejecutamos ambos comandos, vemos que nos sacará valores por consola:
El valor que devuelve el primer comando, para la CPU, debemos dividirlo entre 1000. En mi caso vemos que la CPU de mi Raspberry, en este momento, es de 42.3ºC.

Por otra parte, el segundo comando ya nos devuelve la temperatura de la GPU directamente en Celsius, y en mi caso está sobre los 42ºC.

¿Alguna forma de simplificar esto? Por supuesto! Una de las mil cosas que podemos hacer es crear un script ejecutable que tenga dentro estas instrucciones, y así no tenemos que recordarlas todo el tiempo 🙂

En primer lugar, navegamos al Home de nuestro usuario:

cd ~


Después, creamos un fichero con extensión .sh con el editor de texto que prefiramos. En mi caso, voy a utilizar nano:

nano temperatura.sh

Una vez abierto el editor, vamos a usar el siguiente código para guardar en variables los valores que nos interesan, y después imprimirlos con la instrucción echo. Incluimos algo de formato para que sea más legible. ¡Ah! Sin olvidar la cabecera en el fichero para que pueda ser ejecutable:

#!/bin/bash
# Guardamos el valor de temperatura de la CPU a una variable
cpuTemp=$(cat /sys/class/thermal/thermal_zone0/temp)
# Guardamos el valor de temperatura de la GPU a una variable
gpuTemp=$(vcgencmd measure_temp)
# Imprimimos con el formato que queramos. Incluimos la fecha
echo "$(date)"
echo "Temperatura CPU = $((cpuTemp/1000))'C"
echo "Temperatura GPU = $gpuTemp"


Ya sólo nos queda guardar el fichero, que dependerá del editor de texto que estáis usando. En el caso de nano, vamos a hacer Control+O (Guardar) + Enter para confirmar, y después Control+X (Salir).

Si todo ha ido bien, ya tendremos nuestro script temperatura.sh creado correctamente. Podemos comprobar que el fichero existe a través del comando ls, que nos mostrará todos los archivos del directorio.

Por último, ya sólo nos faltan dos pasos. El primero es marcar el script como ejecutable, y para ello ejecutamos esta instrucción:

chmod +x temperatura.sh

De esta forma ya podemos ejecutar nuestro script con la instrucción:

./temperatura.sh

Como apunte, tan solo recordar que temperatura.sh es un script que está en la carpeta Home del usuario (pi en mi caso). Si nos encontramos en otra carpeta y ejecutamos el comando ./temperatura.sh, nos va a decir que no se encuentra el fichero ya que no estamos en la carpeta Home. Para no estar escribiendo el path completo, podemos incluir nuestro script en la zona de scripts ejecutables del usuario. Esto se hace a través de enlaces simbólicos de Linux con la siguiente instrucción:

sudo ln -s /home/pi/temperatura.sh /usr/bin

Y de esta forma ya podemos ejecutar temperatura.sh sin importar en qué carpeta estemos, para medir la temperatura en una Raspberry Pi y saber si debemos añadir algún tipo de refrigeración para que la CPU y GPU no se calienten en exceso.
