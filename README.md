# Examen-1-Evaluciando-Jose-inojosa


Crea un repositorio para contestar todo el examen. En este repositorio tiene que contener todos los ficheros que creas oportunos para justificar tus respuestas.

Contesta a las preguntas, justificandolas, en un README.md

### Explica métodos para 'abrir' una consola/shell a un contenedor que se está ejecutando

Se abre ventana de la consola y  ahora con el comando `docker compose exec -it (nombre del contenedor) sh` se abrira una consola del contenedor.

### En el contenedor anterior con que opciones tiene que haber sido arrancado para poder interactuar con las entradas y salidas del contenedor


El contenedor en ejecucion con el comando `docker compose up`

### ¿Cómo sería un fichero docker-compose para que dos contenedores se comuniquen entre si en una red solo de ellos?
  cliente:
    container_name: cliente
    image: alpine:latest
    tty: true
    stdin_open: true
    dns:
      - 172.20.5.1  
    networks:
      bind9_subnet:
        ipv4_address: 172.20.5.2

networks:
  bind9_subnet:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16
          ip_range: 172.20.5.0/24
          gateway: 172.20.5.254

Como se muestra en el ejemplo, se debera crear una red dentro del docker-compose esta sera solo de ellos y se podrian comunicar entre si los contenedores.
    
### ¿Qué hay que añadir al fichero anterior para que un contenedor tenga la IP fija?

services:
  bind9:
    image: internetsystemsconsortium/bind9:9.20 
    container_name: bind9
    ports:
      - "54:53/tcp"  
      - "54:53/udp" 
    networks:
      bind9_subnet:
        ipv4_address: 172.20.5.1
    volumes:
      - ./conf:/home/jose/compose/conf 
      - ./zonas:/home/jose/compose/zonas
    restart: always

En la networks se le definira la IP para que sea fija. Esto sirve a la hora de buscarlo.


### ¿Que comando de consola puedo usar para saber las ips de los contenedores anteriores? Filtra todo lo que puedas la salida.
    
### ¿Cual es la funcionalidad del apartado "ports" en docker compose?

Esto permite el acceso al servicio DNS en el contenedor a través del puerto 54 del host.
    
### ¿Para que sirve el registro CNAME? Pon un ejemplo
    
### ¿Como puedo hacer para que la configuración de un contenedor DNS no se borre 
   
    ¿si creo otro contenedor?

Añade una zona tiendadeelectronica.int en tu docker DNS que tenga
        www a la IP 172.16.0.1
        owncloud sea un CNAME de www
        un registro de texto con el contenido "1234ASDF"
        Comprueba que todo funciona con el comando "dig"
        Muestra en los logs que el servicio arranca correctamente
        Realiza el apartado 9 en la máquina virtual con DNS

