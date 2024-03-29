# Instalacion de Docker en RaspberryPi y configuracion inicial


Documetacion sobre la instalacion https://docs.docker.com/engine/install/ubuntu/

Instalación de Docker valida para Raspberrypi 64bits

Configuración del repositorio
```
sudo apt-get update 
sudo apt-get install \
apt-transport-https \
   ca-certificates \
   curl \
   gnupg-agent \
   software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Añadimos Repositorio para arm64 
```
sudo add-apt-repository \
"deb [arch=arm64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
```
Instalar docker
```
sudo apt-get update 
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
Instalar docker compose

```
sudo apt-get install docker-compose
```
Ahora añadimos los permisos necesarios al usuario que tenemos activo para poder usar docker:
```
sudo groupadd docker 
sudo usermod -aG docker $USER
```
lo probamos 
```
docker ps
```
si sale el error:
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json: dial unix /var/run/docker.sock: connect: permission denied

```
sudo chmod 666 /var/run/docker.sock
```





Instalacion de Docker portainer
https://documentation.portainer.io/v2.0/deploy/ceinstalldocker/

```
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

```


Instalacion desde docker-compose


```
version: "3"
services:
    portainer:
      image: portainer/portainer
      container_name: portainer

      ports:
          - 8000:8000
          - 9000:9000

      volumes:
         - /var/run/docker.sock:/var/run/docker.sock
         - portainer_data:/data
      restart: always


volumes:
  portainer_data:
```




Mover archivos docker a otra particion.

```
mv /var/lib/docker /servidor/var/lib/docker
ln -s /servidor/var/lib/docker /var/lib/docker

```
