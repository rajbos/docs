---
title: Opciones de configuración para actualizaciones de dependencias
intro: 'La información detallada para todas las opciones que puedes utilizar para personalizar como el {% data variables.product.prodname_dependabot %} mantiene tus repositorios.'
permissions: 'Las personas con permisos de escritura en un repositorio pueden configurar {% data variables.product.prodname_dependabot %} para el mismo.'
versions:
  free-pro-team: '*'
redirect_from:
  - /github/administering-a-repository/configuration-options-for-dependency-updates
---
{% data reusables.dependabot.beta-note %}

### Acerca del archivo *dependabot.yml*

El archivo de configuración del {% data variables.product.prodname_dependabot %}, *dependabot.yml*, utiliza la sintaxis YAML. Si eres nuevo en YAML y deseas conocer más, consulta "[Aprender YAML en cinco minutos](https://www.codeproject.com/Articles/1214409/Learn-YAML-in-five-minutes)".

Debes almacenar este archivo en el directorio `.github` de tu repositorio. Cuando agregas o actualizas el archivo *dependabot.yml*, esto activa una revisión inmediata de las actualizaciones de la versión. Cualquier opción que también afecte a las actualizaciones de seguridad se utilizará en la siguiente ocasión que una alerta de seguridad active una solicitud de extracción para una alerta de seguridad. Para obtener más información, consulta las secciónes "[Habilitar e inhabilitar las actualizaciones de versión](/github/administering-a-repository/enabling-and-disabling-version-updates)" y "[Configurar las {% data variables.product.prodname_dependabot_security_updates %}](/github/managing-security-vulnerabilities/configuring-github-dependabot-security-updates)".

### Opciones de configuración para *dependabot.yml*

El archivo *dependabot.yml* debe comenzar con `version: 2`, seguido de un conjunto de `updates`.

| Opción                                                                     | Requerido | Descripción                                                                                   |
|:-------------------------------------------------------------------------- |:---------:|:--------------------------------------------------------------------------------------------- |
| [`package-ecosystem`](#package-ecosystem)                                  |   **X**   | Administrador de paquetes a utilizar                                                          |
| [`directorio`](#directory)                                                 |   **X**   | Ubicación de los manifiestos del paquete                                                      |
| [`schedule.interval`](#scheduleinterval)                                   |   **X**   | Qué tan a menudo se revisará si hay actualizaciones                                           |
| [`allow`](#allow)                                                          |           | Personalizar qué actualizaciones se permitirán                                                |
| [`asignatarios`](#assignees)                                               |           | Los asignados a configurar en las solicitudes de extracción                                   |
| [`commit-message`](#commit-message)                                        |           | Preferencias de mensaje de confirmación                                                       |
| [`ignore`](#ignore)                                                        |           | Ignorar ciertas dependencias o versiones                                                      |
| [`etiquetas`](#labels)                                                     |           | Las etiquetas a configurar en las solicitudes de extracción                                   |
| [`hito`](#milestone)                                                       |           | Hito a configurar en las solicitudes de extracción                                            |
| [`open-pull-requests-limit`](#open-pull-requests-limit)                    |           | Limitar la cantidad de solicitudes de extracción abiertas para las actualizaciones de versión |
| [`pull-request-branch-name.separator`](#pull-request-branch-nameseparator) |           | Cambiar el separador para los nombres de rama de la solicitud de extracción                   |
| [`rebase-strategy`](#rebase-strategy)                                      |           | Inhabilitar el rebase automático                                                              |
| [`reviewers`](#reviewers)                                                  |           | Los revisores a configurar en las solicitudes de extracción                                   |
| [`schedule.day`](#scheduleday)                                             |           | Día de la semana para revisar si hay actualizaciones                                          |
| [`schedule.time`](#scheduletime)                                           |           | Hora del día para revisar si hay actualizaciones (hh:mm)                                      |
| [`schedule.timezone`](#scheduletimezone)                                   |           | Huso horario para la hora del día (identificador de zona)                                     |
| [`target-branch`](#target-branch)                                          |           | Rama contra la cual se creará la solicitud de extracción                                      |
| [`versioning-strategy`](#versioning-strategy)                              |           | Cómo actualizar los requisitos de la versión del manifiesto                                   |

Estas opciones caen a groso modo en las siguientes categorías.

- Opciones de configuración esenciales que debes incluir en todas las configuraciones: [`package-ecosystem`](#package-ecosystem), [`directory`](#directory),[`schedule.interval`](#scheduleinterval).
- Opciones para personalizar el calendario de actualización: [`schedule.time`](#scheduletime), [`schedule.timezone`](#scheduletimezone), [`schedule.day`](#scheduleday).
- Opciones para controlar qué dependencias se actualizan: [`allow`](#allow), [`ignore`](#ignore).
- Opciones para agregar metadatos a las solicitudes de extracción: [`reviewers`](#reviewers), [`assignees`](#assignees), [`labels`](#labels), [`milestone`](#milestone).
- Opciones para cambiar el comportamiento de las solicitudes de extracción: [`target-branch`](#target-branch), [`versioning-strategy`](#versioning-strategy), [`commit-message`](#commit-message), [`rebase-strategy`](#rebase-strategy), [`pull-request-branch-name.separator`](#pull-request-branch-nameseparator).

Adicionalmente, la opción [`open-pull-requests-limit`](#open-pull-requests-limit) cambia la cantidad máxima de solicitudes de extracción para las actualizaciones de versión que puede abrir el {% data variables.product.prodname_dependabot %}.

{% note %}

**Nota:** Algunas de estas opciones de configuración también pueden afectar a las solicitudes de extracción que se levantan para las actualizaciones de seguridad de los manifiestos delos paquetes vulnerables.

Las actualizaciones de seguridad se levantan para los manifiestos de paquetes vulnerables únicamente en la rama predeterminada. Cuando se establecen las opciones de configuración para la misma rama (como "true" a menos de que utilices `target-branch`), y se especifica un `package-ecosystem` y `directory` para el manifiesto vulnerable, entonces las solicitudes de extracción para las actualizaciones de seguridad utilizan las opciones relevantes.

En general, las actualizaciones de seguridad utilizan cualquier opción de configuración que afecte las solicitudes de extracción, por ejemplo, agregar metadatos o cambiar su comportamiento. Para obtener más información acerca de las actualizaciones de seguridad, consulta la sección "[Configurar {% data variables.product.prodname_dependabot_security_updates %}](/github/managing-security-vulnerabilities/configuring-github-dependabot-security-updates)".

{% endnote %}

### `package-ecosystem`

**Requerido** Agregas un elemento de `package-ecosystem` para cada administrador de paquetes que quieras que el {% data variables.product.prodname_dependabot %} monitoree para revisar si hay nuevas versiones. El repositorio también debe contener un manifiesto de dependencia o archivo fijado de estos administradores de paquete.

{% data reusables.dependabot.supported-package-managers %}

```yaml
# Basic set up for three package managers

version: 2
updates:

  # Maintain dependencies for GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"

  # Maintain dependencies for npm
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"

  # Maintain dependencies for Composer
  - package-ecosystem: "composer"
    directory: "/"
    schedule:
      interval: "daily"
```

### `directorio`

**Requerido** Debes definir la ubicación de los manifiestos de paquete para cada administrador de paquete (por ejemplo, el *package.json* o *Gemfile*). Tú definierás el directorio relativo a la raíz del repositorio para todos los ecosistemas, menos para GitHub Actions. Para GitHub Actions, configura el directorio para que sea `/` y así revisar los archivos de flujo de trabajo en `.github/workflows`.

```yaml
# Specify location of manifest files for each package manager

version: 2
updates:
  - package-ecosystem: "composer"
    # Files stored in repository root
    directory: "/"
    schedule:
      interval: "daily"

  - package-ecosystem: "npm"
    # Files stored in `app` directory
    directory: "/app"
    schedule:
      interval: "daily"

  - package-ecosystem: "github-actions"
    # Workflow files stored in the
    # default location of `.github/workflows`
    directory: "/"
    schedule:
      interval: "daily"
```

### `schedule.interval`

**Requerido** Debes definir qué tan a menudo se debe revisar si hay nuevas versiones y levantar solicitudes de extracción para las actualizaciones de versión en cada administrador de paquetes. Predeterminadamente, esto ocurre a las 5am UTC. Para modificar esto, utiliza [`schedule.time`](#scheduletime) and [`schedule.timezone`](#scheduletimezone).

- `daily`—se ejecuta en cada día de la semana, de Lunes a Viernes.
- `weekly`—se ejecuta una vez cada semana. Predeterminadamente, esto ocurre los lunes. Para modificar esto, utiliza [`schedule.day`](#scheduleday).
- `monthly`—se ejecuta una vez al mes. Esto ocurre en el primer día de cada mes.

```yaml
# Set update schedule for each package manager

version: 2
updates:

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      # Check for updates to GitHub Actions every weekday
      interval: "daily"

  - package-ecosystem: "composer"
    directory: "/"
    schedule:
      # Check for updates managed by Composer once a week
      interval: "weekly"
```

### `allow`

{% data reusables.dependabot.default-dependencies-allow-ignore %}

Utiliza la opción `allow` para personalizar qué dependencias se actualizan. Esto no impacta en las actualizaciones de seguridad para las dependencias vulnerables. Puedes utilizar las siguientes opciones:

- `dependency-name`—se utiliza para permitir actualizaciones para las dependencias con nombres coincidentes, opcionalmente, utiliza `*` para empatar cero o más caracteres. Para las dependencias de Java, el formato del atributo `dependency-name` es: `groupId:artifactId`, por ejemplo: `org.kohsuke:github-api`.
- `dependency-type`—utilízalo para permitir actualizaciones para dependencias de tipos específicos.

  | Tipos de dependencia | Administradores de paquete compatibles              | Permitir actualizaciones                                                                                                                            |
  | -------------------- | --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
  | `direct`             | Todas                                               | Todas las dependencias definidas explícitamente.                                                                                                    |
  | `indirect`           | `bundler`, `pip`, `composer`, `cargo`               | Las dependencias de las dependencias directas (también conocidas como sub-dependencias, o dependencias transitorias).                               |
  | `todos`              | Todas                                               | Todas las dependencias definidas explícitamente. Para `bundler`, `pip`, `composer`, `cargo`, también las dependencias de las dependencias directas. |
  | `production`         | `bundler`, `composer`, `mix`, `maven`, `npm`, `pip` | Únicamente las dependencias en el "Grupo de dependencias del producto".                                                                             |
  | `development`        | `bundler`, `composer`, `mix`, `maven`, `npm`, `pip` | Únicamente las dependencias en el "Grupo de dependencias de desarrollo".                                                                            |

```yaml
# Customizing the dependencies to maintain with `allow`

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    allow:
      # Allow updates for Lodash
      - dependency-name: "lodash"
      # Allow updates for React and any packages starting "react"
      - dependency-name: "react*"

  - package-ecosystem: "composer"
    directory: "/"
    schedule:
      interval: "daily"
    allow:
      # Allow both direct and indirect updates for all packages
      - dependency-type: "all"

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    allow:
      # Allow only direct updates for
      # Django and any packages starting "django"
      - dependency-name: "django*"
        dependency-type: "direct"
      # Allow only production updates for Sphinx
      - dependency-name: "sphinx"
        dependency-type: "production"
```

### `asignatarios`

Utiliza `assignees` para especificar a los asignados individuales para todas las solicitudes de extracción levantadas para un administrador de paquete.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Specify assignees for pull requests

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    # Add assignees
    assignees:
      - "octocat"
```

### `commit-message`

Predeterminadamente, el {% data variables.product.prodname_dependabot %} intenta detectar tus preferencias de mensajes de confirmación y utiliza patrones similares. Utiliza la opción`commit-message` para especificar tus preferencias explícitamente.

Opciones compatibles

- `prefix` especifica un prefijo para todos los mensajes de confirmación.
- `prefix-development` especifica un prefijo separado para todos los mensajes de confirmación que actualizan dependencias en el grupo de dependencias de desarrollo. Cuando especificas un valor para esta opción, `prefix` se utiliza únicamente para las actualizaciones a las dependencias en el grupo de dependencias de producción. Esto es compatible con: `bundler`, `composer`, `mix`, `maven`, `npm`, y `pip`.
- `include: "scope"` especifica que cualquier prefijo es sucedido por una lista de dependencias actualizadas en la confirmación.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Customizing commit messages

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    commit-message:
      # Prefix all commit messages with "npm"
      prefix: "npm"

  - package-ecosystem: "composer"
    directory: "/"
    schedule:
      interval: "daily"
    # Prefix all commit messages with "Composer"
    # include a list of updated dependencies
    commit-message:
      prefix: "Composer"
      include: "scope"

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    # Include a list of updated dependencies
    # with a prefix determined by the dependency group
    commit-message:
      prefix: "pip prod"
      prefix-development: "pip dev"
      include: "scope"
```

### `ignore`

{% data reusables.dependabot.warning-ignore-option %}

#### Revisar por preferencias ignoradas existentes

Antes de que agreges una opción de `ignore` al archivo de configuración, revisa si has utilizado previamente cualquiera de los comandos de `@dependabot ignore` en una solicitud de extracción de actualizaciones de seguridad o de versión. El {% data variables.product.prodname_dependabot %} almacena estas preferencias para cada administrador de paquetes centralmente, y esta información se sobreescribe mediante la opción `ignore`. Para obtener más información acerca de los comandos de `@dependabot ignore`, consulta la sección "[Administrar las solicitudes de extracción para las actualizaciones de dependencias](/github/administering-a-repository/managing-pull-requests-for-dependency-updates)".

Puedes revisar si un repositorio ha almacenado las preferencias si buscas dicho repositorio con `"@dependabot ignore" in:comments`. Si revias alguna solicitud de extracción en los resultados, puedes decidir si quieres o no especificar esas dependencias o versiones ignoradas en el archivo de configuración.

#### Especificar dependencias y versiones para ignorar

{% data reusables.dependabot.default-dependencies-allow-ignore %}

Puedes utilizar la opción `ignore` para personalizar qué dependencias se actualizarán. La opción `ignore` es compatible con las siguientes opciones.

- `dependency-name`—se utiliza para ignorar actualizaciones para las dependencias con nombres coincidentes, opcionalmente, utiliza `*` para empatar cero o más caracteres. Para las dependencias de Java, el formato del atributo `dependency-name` es: `groupId:artifactId`, por ejemplo: `org.kohsuke:github-api`.
- `versions`—se utiliza para ignorar versiones o rangos específicos de las versiones. Si quieres definir un rango, utiliza el patrón estándar del administrador de paquetes (por ejemplo: `^1.0.0` para npm, o `~> 2.0` para Bundler).

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Customizing the dependencies to maintain with `ignore`

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    ignore:
      - dependency-name: "express"
        # For Express, ignore all updates for version 4 and 5
        versions: ["4.x", "5.x"]
        # For Loadash, ignore all updates
      - dependency-name: "loadash"
```

{% note %}

Las {% data variables.product.prodname_dependabot_version_updates %} no pueden ejecutar actualizaciones de versiones para ninguna dependencia en los manifiestos que contengan dependencias privadas de git o registros privados de git, aún si agregas las dependencias privadas a la opción de `ignore` en tu archivo de configuración. Para obtener más información, consulta la sección "[Acerca de {% data variables.product.prodname_dependabot_version_updates %}](/github/administering-a-repository/about-github-dependabot#supported-repositories-and-ecosystems)".

{% endnote %}

### `etiquetas`

{% data reusables.dependabot.default-labels %}

Utiliza `labels` para anular las etiquetas predeterminadas y especificar las etiquetas alternas para todas las solicitudes de extracción que se levante para un administrador de paquete. Si ninguna de estas etiquetas se define en el repositorio, entonces se ha ignorado. Para inhabilitar todas las etiquetas, incluyendo aquellas predeterminadas, utiliza `labels: [ ]`.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Specify labels for pull requests

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    # Specify labels for npm pull requests
    labels:
      - "npm"
      - "dependencies"
```

### `hito`

Utiliza `milestone` para asociar todas las solicitudes de extracción que se han levantado para un administrador de paquete con un hito. Necesitas especificar el identificador numérico del hito y, no así, su etiqueta. Si ves un hito, la parte final de la URL de la página, después de `milestone`, es el identificador. Por ejemplo: `https://github.com/<org>/<repo>/milestone/3`.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Specify a milestone for pull requests

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    # Associate pull requests with milestone "4"
    milestone: 4
```

### `open-pull-requests-limit`

Predeterminadamente, {% data variables.product.prodname_dependabot %} abre un máximo de cinco solicitudes de extracción para las actualizaciones de versión. Una vez que haya cinco solicitudes de extracción abiertas, las nuevas solicitudes se bloquearán hasta que fusiones o cierres alguna de las que están abiertas. Utiliza `open-pull-requests-limit` para cambiar este límite. Esto también proporciona una forma simple de inhabilitar temporalmente las actualizaciones de versión para un administrador de paquete.

Esta opción no tiene impacto en las actualizaciones de seguridad que tienen un límite separado e interno de diez solicitudes de extracción abiertas.

```yaml
# Changing the number of open pull requests allowed

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    # Disable version updates for npm dependencies
    open-pull-requests-limit: 0

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    # Allow up to 10 open pull requests for pip dependencies
    open-pull-requests-limit: 10
```

### `pull-request-branch-name.separator`

El {% data variables.product.prodname_dependabot %} genera una rama para cada solicitud de extracción. Cada nombre de rama incluye `dependabot`, y el administrador de paquete y la dependencia que se actualizaron. Predeterminadamente, estas partes están separadas por un símbolo de `/`, por ejemplo: `dependabot/npm_and_yarn/next_js/acorn-6.4.1`.

Utiliza `pull-request-branch-name.separator` para especificar un separador diferente. Este puede ser alguno de entre: `"-"`, `_` o `/`. El símbolo de guión debe estar entre comillas porque, de lo contrario, se interpretará como que está declarando una lista YAML vacía.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Specifying a different separator for branch names

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    pull-request-branch-name:
      # Separate sections of the branch name with a hyphen
      # for example, `dependabot-npm_and_yarn-next_js-acorn-6.4.1`
      separator: "-"
```

### `rebase-strategy`

Predeterminadamente, el {% data variables.product.prodname_dependabot %} rebasa automáticamente las solicitudes de extracción abiertas cuando detecta conflictos. Utiliza `rebase-strategy` para inhabilitar este comportamiento.

Estrategias de rebase disponibles

- `disabled` para inhabilitar el rebase automático.
- `auto` para utilizar el comportamiento predeterminado y rebasar las solicitudes de extracción abiertas cuando se detecten conflictos.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Disabling automatic rebasing

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    # Disable rebasing for npm pull requests
    rebase-strategy: "disabled"
```

### `reviewers`

Utiliza `reviewers` para especificar los revisores o equipos individuales de revisores para las solicitudes de extracción que se levantaron para un administrador de paquete. Debes utilizar el nombre completo del equipo, incluyendo la organización, como si lo estuvieras @mencionando.

{% data reusables.dependabot.option-affects-security-updates %}

```yaml
# Specify reviewers for pull requests

version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    # Add reviewers
    reviewers:
      - "octocat"
      - "my-username"
      - "my-org/python-team"
```

### `schedule.day`

Cuando configuras un calendario de actualización en `weekly`, predeterminadamente, el {% data variables.product.prodname_dependabot %} revisa las nuevas versiones los lunes a las 05:00 UTC. Utiliza `schedule.day` para especificar un día alterno para revisar si hay actualizaciones.

Valores compatibles

- `monday`
- `tuesday`
- `wednesday`
- `thursday`
- `friday`
- `saturday`
- `sunday`

```yaml
# Specify the day for weekly checks

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      # Check for npm updates on Sundays
      day: "sunday"
```

### `schedule.time`

Predeterminadamente, el {% data variables.product.prodname_dependabot %} revisa si hay nuevas versiones a las 05:00 UTC. Utiliza `schedule.time` para especificar una hora alterna para revisar si hay actualizaciones (formato: `hh:mm`).

```yaml
# Set a time for checks
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
      # Check for npm updates at 9am UTC
      time: "09:00"
```

### `schedule.timezone`

Predeterminadamente, el {% data variables.product.prodname_dependabot %} revisa si hay nuevas versiones a las 05:00 UTC. Utiliza `schedule.timezone` para especificar un huso horario alternativo. El identificador de zona debe ser tomado de la base de datos de Husos Horarios que mantiene [iana](https://www.iana.org/time-zones). Para obtener más información, consulta la [Lista de bases de datos tz para husos horarios](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

```yaml
# Specify the timezone for checks

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
      time: "09:00"
      # Use Japan Standard Time (UTC +09:00)
      timezone: "Asia/Tokyo"
```

### `target-branch`

Predeterminadamente, el {% data variables.product.prodname_dependabot %} revisa si hay archivos de manifiesto en las ramas predeterminadas y levanta solicitudes de extracción para las actualizaciones de versión contra dicha rama. Utiliza `target-branch` para especificar una rama diferente para los archivos de manifiesto y para las solicitudes de extracción. Cuando utilizas esta opción, la configuración para este administrador de paquete ya no afectará ninguna solicitud de extracción que se haya levantado para las actualizaciones de seguridad.

```yaml
# Specify a non-default branch for pull requests for pip

version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    # Raise pull requests for version updates
    # to pip against the `develop` branch
    target-branch: "develop"
    # Labels on pull requests for version updates only
    labels:
      - "pip dependencies"

  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      # Check for npm updates on Sundays
      day: "sunday"
    # Labels on pull requests for security and version updates
    labels:
      - "npm dependencies"
```

### `versioning-strategy`

Cuando el {% data variables.product.prodname_dependabot %} edita un archivo de manifiesto para actualizar una versión, utiliza las siguientes estrategias generales:

- Para las apps, los requisitos de versión se incrementan, por ejemplo: npm, pip y Composer.
- Para las bibliotecas, el rango de versiones se amplía, por ejemplo: Bundler y Cargo.

Utiliza la opción `versioning-strategy` para cambiar este comportamiento para los administradores de paquete compatibles.

{% data reusables.dependabot.option-affects-security-updates %}

Estrategias de actualización disponibles

| Opción                  | Compatible con                                      | Acción                                                                                                                                                                      |
| ----------------------- | --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `lockfile-only`         | `bundler`, `cargo`, `composer`, `mix`, `npm`, `pip` | Crear únicamente solicitudes de extracción para actualizar los archivos fijados. Ignorar cualquier versión nueva que pudiera requerir cambios en el paquete del manifiesto. |
| `auto`                  | `bundler`, `cargo`, `composer`, `mix`, `npm`, `pip` | Seguir la estrategia predeterminada descrita anteriormente.                                                                                                                 |
| `widen`                 | `composer`, `npm`                                   | Relajar el requisito de versión para que incluya tanto la versión nueva como la anterior, cuando sea posible.                                                               |
| `increase`              | `bundler`, `composer`, `npm`                        | Siempre incrementar el requisito de versión para que empate con la versión nueva.                                                                                           |
| `increase-if-necessary` | `bundler`, `composer`, `npm`                        | Incrementar el requisito de versión únicamente cuando lo requiera la versión nueva.                                                                                         |

```yaml
# Customizing the manifest version strategy

version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    # Update the npm manifest file to relax
    # the version requirements
    versioning-strategy: widen

  - package-ecosystem: "composer"
    directory: "/"
    schedule:
      interval: "daily"
    # Increase the version requirements for Composer
    # only when required
    versioning-strategy: increase-if-necessary

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    # Only allow updates to the lockfile for pip and
    # ignore any version updates that affect the manifest
    versioning-strategy: lockfile-only
```
