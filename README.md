# Ansible Script

## Descripción
Este script de Ansible está diseñado para cargar datos de un archivo `data.yml` y ejecutar tareas relacionadas con grupos específicos.

## Requisitos
- Ansible instalado en la máquina local o el servidor donde se ejecutará este script.
- GitHub CLI instalado y configurado
  - [Descarga](https://cli.github.com/)
  - [Documentacion](https://cli.github.com/)

## Uso
1. Asegúrate de tener Ansible instalado.
2. Clona o descarga este repositorio en la máquina desde la que planeas ejecutar el script de Ansible.
3. Ajusta el archivo `data.yml` con los datos necesarios.
4. Verifica la configuración en el script Ansible, en particular:
   - `file: files/minidata.yml` para cargar el archivo de datos.
   - `group: 5` para definir el grupo específico que se desea usar.
5. Asegurate de tener el archivo secrets_file.enc
    ```text
      gh_mirror_username: "gh user"
      gh_mirror_token: "gh pat"
      bbs_token: "bbs token"
      gh_token: "gh pat"
      gh_username: "gh user"

    ```
    si deseas encryptar el archivo puedes usar este comando:
    ```bash
      ansible-vault encrypt secrets_file.enc
    ```
6. Ejecuta el script Ansible en la máquina local o el servidor usando el siguiente comando:

   ```bash
    ansible-playbook -e @secrets_file.enc --ask-vault-pass main.yml -vvv
 ## Detalles del script

- **hosts:** `localhost`
  - El script se ejecutará localmente en la máquina donde se inicia.

- **vars:**
  - `group: 5`: Define el grupo que se utilizará para filtrar los datos.

### Tareas

1. **Load data from data.yml**
   - **include_vars:** `file: files/minidata.yml`
     - Carga los datos desde el archivo `minidata.yml`.

2. **Include tasks from git.yml**
   - **loop:** `{{ data_file.data }}`
     - Itera sobre los datos cargados.
   - **when:** `item.grupo == group`
     - Condición para ejecutar las tareas cuando el grupo coincide con la variable definida.
   - **loop_control:**
     - `loop_var: item`: Nombre de la variable de iteración.

## Notas adicionales

- Asegúrate de revisar y personalizar los archivos `data.yml`, `minidata.yml` y `git.yml` según las necesidades del proyecto.
- Este script asume una estructura específica de datos; verifica que tus archivos estén formateados correctamente.
