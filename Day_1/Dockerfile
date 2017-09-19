# Imagen Base
# Calculate Pi with montecarlo
FROM gcc:4.9

# Datos sobre el propietario de la imagen
MAINTAINER Eduardo Arango <carlos.arango.gutierrez@correounivalle.edu.co>

# Copiar archivos necesarios para correr la aplicacion
COPY . /usr/src/myapp

# Cambiar de directorio (Por defecto se esta en /)
WORKDIR /usr/src/myapp

# Se Compila el binario de la aplicacion
RUN gcc pi.c -lm -o pi.o
RUN cp pi.o /bin

# Comando a ejecutar al correr el contenedor
ENTRYPOINT ["pi.o"]
