---
title: Administrar las alertas del escaneo de código para tu repositorio
shortTitle: Administrar alertas
intro: 'Desde la vista de seguridad, puedes ver, corregir, descartar o borrar las alertas de las vulnerabilidades o errores potenciales en el código de tu proyecto.'
product: '{% data reusables.gated-features.code-scanning %}'
permissions: 'If you have write permission to a repository you can manage {% data variables.product.prodname_code_scanning %} alerts for that repository.'
versions:
  free-pro-team: '*'
  enterprise-server: '>=3.0'
  github-ae: '*'
redirect_from:
  - /github/managing-security-vulnerabilities/managing-alerts-from-automated-code-scanning
  - /github/finding-security-vulnerabilities-and-errors-in-your-code/managing-alerts-from-code-scanning
  - /github/finding-security-vulnerabilities-and-errors-in-your-code/managing-code-scanning-alerts-for-your-repository
  - /code-security/secure-coding/managing-code-scanning-alerts-for-your-repository
type: how_to
topics:
  - Advanced Security
  - Code scanning
  - Alerts
  - Repositories
---

<!--For this article in earlier GHES versions, see /content/github/finding-security-vulnerabilities-and-errors-in-your-code-->

{% data reusables.code-scanning.beta %}

### Acerca de las alertas de {% data variables.product.prodname_code_scanning %}

Puedes configurar el {% data variables.product.prodname_code_scanning %} para que verifique el código en un repositorio utilizando el análisis predeterminado de {% data variables.product.prodname_codeql %}, un análisis de terceros, o varios tipos de análisis. Cuando se complete el análisis, las alertas resultantes se mostrarán unas junto a otras en la vista de seguridad del repositorio. Los resultados de las herramientas de terceros o de las consultas personalizadas podrían no incluir todas las propiedades que ves para las alertas que se detectan con el análisis predeterminado del {% data variables.product.prodname_codeql %} de {% data variables.product.company_short %}. Para obtener más información, consulta la sección "[Configurar el {% data variables.product.prodname_code_scanning %} en un repositorio](/code-security/secure-coding/setting-up-code-scanning-for-a-repository)".

Predeterminadamente, el {% data variables.product.prodname_code_scanning %} analiza tu código periódicamente en la rama predeterminada y durante las solicitudes de cambios. Para obtener información acerca de la administración de alertas en una solicitud de cambios, consulta la sección "[Clasificar las alertas del {% data variables.product.prodname_code_scanning %} en las solicitudes de cambios](/code-security/secure-coding/triaging-code-scanning-alerts-in-pull-requests)".

{% data reusables.code-scanning.upload-sarif-alert-limit %}

### Acerca de los detalles de las alertas

Cada alerta resalta un problema en el código y el nombre de la herramienta que lo identificó. Puedes ver la línea de código que activó la alerta, así como las propiedades de la misma, tales como la severidad y la naturaleza de dicho problema. Las alertas también te dicen si el problema se introdujo por primera vez. Para las alertas que identificó el análisis de {% data variables.product.prodname_codeql %}, también verás información de cómo arreglar elproblema.

![Ejemplo de alerta de {% data variables.product.prodname_code_scanning %}](/assets/images/help/repository/code-scanning-alert.png)

Si configuras el {% data variables.product.prodname_code_scanning %} utilizando {% data variables.product.prodname_codeql %}, esto también puede detectar problemas de flujo de datos en tu código. El análisis de flujo de datos encuentra problemas de seguridad potenciales en el código, tales como: utilizar los datos de formas no seguras, pasar argumentos peligrosos a las funciones y tener fugas de información sensible.

Cuando {% data variables.product.prodname_code_scanning %} reporta alertas de flujo de datos, {% data variables.product.prodname_dotcom %} te muestra como se mueven los datos a través del código. El {% data variables.product.prodname_code_scanning_capc %} te permite identificar las áreas de tu código que filtran información sensible y que podrían ser el punto de entrada para los ataques que hagan los usuarios malintencionados.

### Visualizar las alertas de un repositorio

Cualquiera con permisos de escritura en un repositorio puede ver las anotaciones del {% data variables.product.prodname_code_scanning %} en las solicitudes de cambios. Para obtener màs informaciònPara obtener más información, consulta la sección "[Clasificar las alertas del {% data variables.product.prodname_code_scanning %} en las solicitudes de extracción](/code-security/secure-coding/triaging-code-scanning-alerts-in-pull-requests)".

Necesitas permisos de escritura para ver un resumen de todas las alertas de un repositorio en la pestaña de **Seguridad**. Predeterminadamente, las alertas se muestran para la rama predeterminada.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-security %}
{% data reusables.repositories.sidebar-code-scanning-alerts %}
{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.1"%}
1. Optionally, use{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.2" %} the free text search box or{% endif %} the drop-down menus to filter alerts. Por ejemplo, puedes filtrar por la herramienta que se utilizó para identificar las alertas. ![Filter by tool](/assets/images/help/repository/code-scanning-filter-by-tool.png){% endif %}
1. Debajo de "{% data variables.product.prodname_code_scanning_capc %}", da clic en la alerta que quisieras explorar.
{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.1"%}
  ![Resumen de alertas](/assets/images/help/repository/code-scanning-click-alert.png)
{% else %}
  ![Lista de alertas de {% data variables.product.prodname_code_scanning %}](/assets/images/enterprise/3.1/help/repository/code-scanning-click-alert.png)
{% endif %}
1. Opcionalmente, si la alerta resalta un problema con el flujo de datos, da clic en **Mostrar rutas** para mostrar la ruta desde la fuente de datos hacia el consumidor de datos en donde se utiliza. ![El enlace de "Mostrar rutas" en una alerta](/assets/images/help/repository/code-scanning-show-paths.png)
1. Las alertas del análisis de {% data variables.product.prodname_codeql %} incluyen una descripción del problema. Da clic en **Mostrar más** para obtener orientación sobre cómo arreglar tu código. ![Detalles de una alerta](/assets/images/help/repository/code-scanning-alert-details.png)

{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.2" %}
### Searching {% data variables.product.prodname_code_scanning %} alerts

You can search the list of alerts. This is useful if there is a large number of alerts in your repository, or if you don't know the exact name for an alert for example. {% data variables.product.product_name %} performs the free text search across:
- The name of the alert
- The alert description
- The alert details (this also includes the information hidden from view by default in the **Show more** collapsible section)

 ![The alert information used in searches](/assets/images/help/repository/code-scanning-free-text-search-areas.png)

| Supported search                           | Syntax example      | Results                                                            |
| ------------------------------------------ | ------------------- | ------------------------------------------------------------------ |
| Single word search                         | `injection`         | Returns all the alerts containing the word `injection`             |
| Multiple word search                       | `sql injection`     | Returns all the alerts containing `sql` or `injection`             |
| Exact match search</br>(use double quotes) | `"sql injection"`   | Returns all the alerts containing the exact phrase `sql injection` |
| OR search                                  | `sql OR injection`  | Returns all the alerts containing `sql` or `injection`             |
| AND search                                 | `sql AND injection` | Returns all the alerts containing both words `sql` and `injection` |

{% tip %}

**Tips:**
- The multiple word search is equivalent to an OR search.
- The AND search will return results where the search terms are found _anywhere_, in any order in the alert name, description, or details.

{% endtip %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-security %}
{% data reusables.repositories.sidebar-code-scanning-alerts %}
1. To the right of the **Filters** drop-down menus, type the keywords to search for in the free text search box. ![The free text search box](/assets/images/help/repository/code-scanning-search-alerts.png)
2. Press <kbd>return</kbd>. The alert listing will contain the open {% data variables.product.prodname_code_scanning %} alerts matching your search criteria.

{% endif %}

### Arreglar una alerta

Cualquiera con permisos de escritura en un repositorio puede arreglar una alerta si confirma una corrección en el código. Si el repositorio tiene programado un {% data variables.product.prodname_code_scanning %} para ejecutarse en las solicitudes de cambios, es mejor levantar una solicitud de cambios con tu corrección. Esto activará el análisis del {% data variables.product.prodname_code_scanning %} en los cambios y probará que tu arreglo no introduciría ningún problema nuevo. Para obtener más información, consulta la sección "[Configurar el {% data variables.product.prodname_code_scanning %}](/code-security/secure-coding/configuring-code-scanning)" y "[Clasificar las alertas del {% data variables.product.prodname_code_scanning %} en las solicitudes de cambios](/code-security/secure-coding/triaging-code-scanning-alerts-in-pull-requests)".

Si tienes permisos de escritura para un repositorio, puedes ver las alertas arregladas si ves el resumen de las alertas y das clic en **Cerrado**. Para obtener más información, consulta la sección "[Visualizar las alertas de un repositorio](#viewing-the-alerts-for-a-repository)". La lista de "Cerrado" muestra las alertas arregladas y las que los usuarios han descartado.

You can use{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.2" %} the free text search or{% endif %} the filters to display a subset of alerts and then in turn mark all matching alerts as closed.

Las alertas pueden arreglarse en una rama pero no en alguna otra. Puedes utilizar el menú desplegable de "Rama", en el resumen de las alertas, para verificar si una alerta se arregló en una rama en particular.

{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.1"%}
![Filtrar alertas por rama](/assets/images/help/repository/code-scanning-branch-filter.png)
{% else %}
![Filtrar alertas por rama](/assets/images/enterprise/3.1/help/repository/code-scanning-branch-filter.png)
{% endif %}

### Descartar o borrar las alertas

Hay dos formas de cerrar una alerta. Puedes arreglar el problema en el código, o puedes descartar la alerta. Como alternativa, si tienes permisos adminsitrativos en el repositorio, puedes borrar las alertas. Borrar las alertas es útil en situaciones en donde configuraste una herramienta del {% data variables.product.prodname_code_scanning %} y luego decidiste eliminarla, o donde configuraste el análisis de {% data variables.product.prodname_codeql %} con un conjunto más grande de consultas que quieres seguir utilizando y después eliminaste algunas de ellas de la herramienta. En ambos casos, el borrar las alertas te permite limpiar tus resultados del {% data variables.product.prodname_code_scanning %}. Puedes borrar las alertas de la lista de resumen dentro de la pestaña de **Seguridad**.

El descartar una alerta es una forma de cerrar aquellas que no crees que necesiten arreglo. {% data reusables.code-scanning.close-alert-examples %} Puedes eliminar alertas desde las anotaciones del {% data variables.product.prodname_code_scanning %} en el código, o desde la lista de resumen dentro de la pestaña de **Seguridad**.

Cuando descartas una alerta:

- Se descarta en todas las ramas.
- La alerta se elimina de la cantidad de alertas actuales para tu proyecto.
- La alerta se mueve a la lista de "Cerrado" en el resumen de alertas, desde donde puedes volver a abrirla en caso de que lo necesites.
- La razón por la cual cerraste la alerta se registra.
- La siguiente vez que se ejecute el {% data variables.product.prodname_code_scanning %}, este código no volverá a generar una alerta.

Cuando borras una alerta:

- Se borra en todas las ramas.
- La alerta se elimina de la cantidad de alertas actuales para tu proyecto.
- _No_ seagrega a la lista de "Cerrado" en el resumen de las alertas.
- Si el código que generó la alerta se mantiene tal cual, y se ejecuta nuevamente la misma herramienta del {% data variables.product.prodname_code_scanning %} sin ningún cambio de configuración, la alerta se mostrará nuevamente en los resultados de tu análisis.

Para descartar o borrar una alerta:

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-security %}
{% data reusables.repositories.sidebar-code-scanning-alerts %}
1. Si tienes permisos administrativos en el repositorio y quieres borrar las alertas para esta herramienta del {% data variables.product.prodname_code_scanning %}, selecciona algunas o todas las casillas de verificación y da clic en **Borrar**.

   ![Borrar alertas](/assets/images/help/repository/code-scanning-delete-alerts.png)

   Optionally, you can use{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.2" %} the free text search or{% endif %} the filters to display a subset of alerts and then delete all matching alerts at once. Por ejemplo, si eliminaste una consulta desde el análisis de {% data variables.product.prodname_codeql %}, puedes utilizar el filtro de "Regla" para listar solo las alertas para esa consulta y luego seleccionar y borrar todas esas alertas.

{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.1"%}
  ![Filtrar alertas por regla](/assets/images/help/repository/code-scanning-filter-by-rule.png)
{% else %}
  ![Filtrar alertas por regla](/assets/images/enterprise/3.1/help/repository/code-scanning-filter-by-rule.png)
{% endif %}

1. Si quieres descartar una alerta, es importante explorarla primero para que puedas elegir la razón correcta para descartarla. Da clic en la alerta que quisieras explorar.

{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@3.1"%}
   ![Abrir una alerta desde la lista de sumario](/assets/images/help/repository/code-scanning-click-alert.png)
{% else %}
  ![Lista de alertas de {% data variables.product.prodname_code_scanning %}](/assets/images/enterprise/3.1/help/repository/code-scanning-click-alert.png)
{% endif %}

1. Revisa la alerta y da clic en **Descartar** y elije una razón para cerrarla. ![Elegir una razón para descartar una alerta](/assets/images/help/repository/code-scanning-alert-close-drop-down.png)

   {% data reusables.code-scanning.choose-alert-dismissal-reason %}

   {% data reusables.code-scanning.false-positive-fix-codeql %}

#### Descartar varias alertas al mismo tiempo

Si un proyecto tiene varias alertas que quieras descartar por la misma razón, puedes descartarlas por lote desde el resúmen de las alertas. Habitualmente quieres filtrar la lista y luego descartar todas las alertas coincidentes. Por ejemplo, puede que quieras descartar todas las alertas actuales del proyecto que se hayan etiquetado para una vulnerabilidad de Enumeración de Debilidades (CWE, por sus siglas en inglés) Común en particular.

### Leer más

- "[Clasificar las alertas del {% data variables.product.prodname_code_scanning %} en las solicitudes de cambios](/code-security/secure-coding/triaging-code-scanning-alerts-in-pull-requests)"
- "[Configurar el {% data variables.product.prodname_code_scanning %} en un repositorio](/code-security/secure-coding/setting-up-code-scanning-for-a-repository)"
- "[Acerca de la integración con {% data variables.product.prodname_code_scanning %}](/code-security/secure-coding/about-integration-with-code-scanning)"
