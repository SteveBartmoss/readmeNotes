# Instalar docker

Se debe descargar el archivo desde el sitio oficial [aqui](https://www.docker.com/products/docker-desktop/) en el caso de que el equipo sea apple silicon se debe seleccionar "Mac with apple silicon" el procedimiento para instalarlo en macOs es el mismo que otras aplicaciones de mac asi que no deberia haber algun otro problema.

## Verificar la instalacion

```php
docker --version 
docker-compose --version
docker run --rm hello-world
```

## Configuraciones recomendadas

### Ajustar recursos (opcional)

- Abrir Docker Desktop

- Ir a Settings > Resources

- Ajustar CPU (4-6 cores suele ser suficiente)

- Memoria (4-8GB o lo que sea necesario)

- Swap (1-2GB)

### Habilitar Kubernetes 

- Settings > Kubernetes

- Marcar "Enable Kubernetes"

- Click en "Apply & Restart"

## Consideraciones para Apple Silicon

- **Imagenes multi-architecture:** De manera predeterminada docker manejara imagenes ARM y x86, pero si queremos tener un mejor rendimiento debemos buscar imagenes con soporte ARM/Apple Silicon:

    ```bash
    docker pull --platform linux/arm64 nombre-imagen
    ```

- **Problemas comunes:** Algunas imagenes no tienen version ARM. Se puede forzar una emulacion x86 con el siguiente comando
    ```bash
    docker run --platform linux/amd64 nombre-imagen
    ```
    Esto es mas lento pero funcional si no se encuentra una alternativa ARM

### Primeros pasos despues de instalar

Se puede probar la instalacion de docker con los siguientes comandos

```bash
# Ejecutar un contenedor Nginx para ARM
docker run --rm -d -p 8080:80 --name nginx-arm nginx:alpine

# Ver contenedores en ejecucion
docker ps

# Detener el contenedor 
docker stop nginx-arm
```