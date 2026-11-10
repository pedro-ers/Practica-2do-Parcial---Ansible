# Laboratorio de Ansible con Docker

## Objetivo

Implementar un laboratorio de automatización utilizando Ansible y Docker, administrando dos contenedores Ubuntu 24.04 desde un inventario y un playbook.

El laboratorio permite practicar la ejecución de tareas automatizadas, recopilación de información del sistema, creación de directorios y archivos, uso de ciclos (`loop`) y condiciones (`when`).

## Tecnologías utilizadas

- Ubuntu 24.04
- Docker y Docker Compose
- Ansible
- Python 3

## Estructura del proyecto

```text
ansible-lab/
├── docker-compose.yml
├── inventory.ini
├── playbook.yml
└── README.md
```

## Iniciar los contenedores

Ejecutar el siguiente comando desde la carpeta del proyecto:

```bash
docker compose up -d
```

Verificar que ambos contenedores estén activos:

```bash
docker ps
```

## Preparar los servidores

Instalar Python 3 en cada contenedor:

```bash
docker exec server1 bash -c "apt update && apt install -y python3"
docker exec server2 bash -c "apt update && apt install -y python3"
```

## Verificar la conectividad

Ejecutar la prueba de conectividad de Ansible:

```bash
ansible docker -i inventory.ini -m ansible.builtin.ping
```

Ambos servidores deben responder correctamente con `SUCCESS` y `pong`.

## Ejecutar el playbook

Ejecutar:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

El playbook realiza las siguientes tareas:

1. Recopila información de los servidores.
2. Muestra el nombre de cada servidor.
3. Muestra el sistema operativo y su versión.
4. Crea el directorio `/tmp/ansible-demo`.
5. Genera el archivo `info.txt` con información del servidor.
6. Crea los subdirectorios `logs`, `backup` y `config` mediante un ciclo.
7. Muestra un mensaje cuando el sistema operativo es Ubuntu.

## Verificar los resultados

Comprobar la estructura creada en cada contenedor:

```bash
docker exec server1 ls -R /tmp/ansible-demo
docker exec server2 ls -R /tmp/ansible-demo
```

Consultar el contenido de los archivos:

```bash
docker exec server1 cat /tmp/ansible-demo/info.txt
docker exec server2 cat /tmp/ansible-demo/info.txt
```

## Evidencia de ejecución

Agregar aquí una captura de pantalla que muestre la ejecución exitosa del playbook y el resumen final (`PLAY RECAP`).

## Detener los contenedores

Para detener y eliminar los contenedores del laboratorio:

```bash
docker compose down
```
