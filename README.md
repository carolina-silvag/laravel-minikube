# Docker kubernetes minikuber laravel

## Integrantes:

* Carolina Silva
* Andrés Navarro
* Luis Pradenas
* David Arellano

## Introducción

Una aplicación simple realizada en el framework laravel para utilizar docker y kubernetes por medio de la herramienta minikube que facilita su implementación local.

Para los siguientes pasos se utilizó el sistema operativo Mac OS.

## Instalación de docker

`brew cask install docker `

__Comprobar version de docker__

`docker --version `

## Proyecto php con framework Laravel

__Ruta local al proyecto Laravel__

http://laravel-minikube.test/

__clonar proyecto__

`git clone https://github.com/carolina-silvag/laravel-minikube`

__Instalar dependencias con composer__

`composer install`


## Configuración de docker

__archivo dockerfile__

```
FROM composer:1.6.5 as build

WORKDIR /app
COPY . /app
RUN composer install

FROM php:7.1.8-apache

EXPOSE 80
COPY --from=build /app /app
COPY vhost.conf /etc/apache2/sites-available/000-default.conf
RUN chown -R www-data:www-data /app \
    && a2enmod rewrite
```

## Creación contenedor de docker

__ingresar al directorio del proyecto__

`cd ruta/al/proyecto/`

__construir contenedor__

`docker build -t laravel-minikube:v1 .`

__revisar imagenes de docker__

`docker images`

## Ejecutar el contenedor de docker

__iniciar contenedor docker__

`docker run -ti -p 8080:80  -e APP_KEY=base64:cUPmwHx4LXa4Z25HhzFiWCf7TlQmSqnt98pnuiHmzgY= laravel-minikube:v1`

__ruta proyecto docker__

`http://localhost:8080`


## Instalar kubernetes

__instalación por brew__

`brew install kubectl`

__conmprobar verisión__

`kubectl version --client `

## Instalar minikube

__instalación minikube con brew__

`brew install minikube`

__version minikube__

`minikube version `

__iniciar minikube__

`minikube start --driver=docker`


## construir contenedor docker en minikube

`eval $(minikube docker-env)`

`docker build -t laravel-minikube:v1 .`

## Desplegando kubernetes

`kubectl config use-context minikube`

`kubectl run laravel-minikube --image=laravel-minikube:v1 --port=80 --image-pull-policy=IfNotPresent --env=APP_KEY=base64:cUPmwHx4LXa4Z25HhzFiWCf7TlQmSqnt98pnuiHmzgY=`


__verificación de pods__

`kubectl get pods`

__verificar en el dashboard__

`minikube dashboard`


## Exponiendo la aplicación

__creamos un servicio__

`kubectl expose pods laravel-minikube --type=NodePort --port=80`

__ver los servicios__

`kubectl get services`

__ver aplicación desde minikube__

`minikube service laravel-minikube`


## Escalando la aplicación

__Creamos un deployments__

`kubectl create deployment laravel-minikube --image=laravel-minikube:v1`

__revisamos los deployment__

`kubectl get deployments`

__escalamos a 3 pods el deployment__

`kubectl scale --replicas=3 deployment.app/laravel-minikube`

