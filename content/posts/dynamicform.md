---
title: "Creación de formularios dinámicos en Django"
date: 2021-04-17T18:25:38+01:00
draft: true
---

# Introducción

 Quise escribir esta entrada sobre como crear formularios dinámicos por ser un problema que yo mismo he encontrado mientras trabajaba en una webapp. Esta webapp debía tener un formulario para introducir ~30 variables clínicas y proporcionar predicciones por tres modelos distintos (Referencia: Nuestro preprint). El principal problema es que la selección de variables para cada modelo no estaba determinada, por tanto no podía confiar en tener siempre las mismas variables.

Cuando busqué como hacer esto, porque como posiblemente ya hayáis deducido soy un melón para el desarrollo web, me di cuenta de que no habían buenos recursos que explicaran justo lo que yo necesitaba. Así que hoy toca como crear un formulario dínamico en Django (+bootstrap)

# Antes de empezar

 Este tutorial está pensado para gente con una experiencía mínima en Django, es decir, que haya trasteado un poco con las diferentes partes: las aplicaciones, la parte de URLs, los formularios, los modelos y el ORM...

Si de todo esto no te suena ni papa te recomiendo empezar por el tutorial oficial de Django: https://docs.djangoproject.com/en/3.2/intro/tutorial01/

# La clase Form

Vamos a utilizar el método standard de Django para la creación de formularios. Basicamente la creación de una clase, preferiblemente en el fichero forms.py de la aplicación pertinente que herede de la clase Form.

```py
  from django import forms
  class SimpleDynamicForm(forms.Form):    
```

 Dentro de esta clase deberemos implementar su método `__init__` que será el responsable de crear el formulario. Dentro de este método deberíamos leer los campos a renderizar desde algún lugar, en nuestro caso utilizamos un csv generado en los notebooks donde realizamos la experimentación de los modelos. Nuestro fichero csv contiene la siguiente información: nombre de la variable (var), tipo de la variable (type [int, float...]), rango de la variable (min y max) y por último el texto que debe mostrar la variable usando la herramienta tooltip. para dar ayuda al usuario.

En la implementación propuesta pasamos un pandas dataframe al formulario con este contenido a través de los `**kwargs`, esta construcción de python, y explicado a groso modo, nos permite pasar los argumentos extra que queramos a la función, lo que es muy útil cuando sobreescribimos la clase Form de la que heredamos.

``` py
def __init__(self, *args, **kwargs):
    super(SimpleDynamicForm, self).__init__(*args)
    # Informacion para construir los campos en forma de DataSet
    fields_df: pd.DataFrame = kwargs.get('fields_df')
    initial: dict = kwargs.pop('initial', {})
```

 Nota: initial es un diccionario que usaríamos en caso de querer crear un formulario ya relleno con algunos valores. Por ejemplo, imaginemos que existe una opción para cargar desde fichero una información rellena previamente. De momento no lo usaremos pero se ha incluído por completitud.

Ahora deberemos iterar por las diferentes filas del dataframe. Repito, en mi caso es un dataframe, podría haber sido un JSON, por ejemplo o cualquier otro tipo de fichero cargado en python. Con la información de cada fila construiremos un campo del formulario. Al final del todo este campo lo asignaremos a self.fields, que es la variable heredada donde se guardan los campos. 

``` py
    for row in fields_df.itertuples():
        # nombre y tipo,
        name_field = row.var
        type_field = row.type
        tooltip_text = row.tooltiptext

        if tooltip_text:
            label_field = f'<‍div data-toggle="tooltip" \
            title="{tooltip_text}"> {name_field} <‍/div>'
        else:
            label_field = name_field

        if type_field == 'int64':
            actual_field = forms.IntegerField(label=label_field,
                                            required=True,
                                            min_value=row.min,
                                            max_value=row.max,
                                            initial=int(initial.get(name_field)) 
                                            if initial.get(name_field) 
                                                is not None else None)

        elif type_field == 'float64':
            actual_field = forms.FloatField(label=label_field,
                                            required=True,
                                            min_value=row.min,
                                            max_value=row.max,
                                            initial=initial.get(name_field),
                                            widget=NumberInput(
                                                attrs={'step': "0.01"}))

        # agregar a self.fields
        self.fields[name_field] = actual_field
```

 Nótese que el nombre del campo corresponde a la variable `name_field`, mientras que `label_field` corresponderá al texto que renderizaremos justo encima del campo, es por esto que editamos esta última variable para añadir el html del tooltip.

Es importante destacar que en este ejemplo solo estábamos trabajando con variables numéricas (int y float). Si se tienen más tipos se deberá añadir nuevos bloques if con los distintos tipos y escribir el código necesario para gestionarlos (p.e. fechas o droplists con categoría). No obstante este es el esquema general básico y ampliable. 

# Template

Finalmente, cuando en él código de la vista creemos el formulario y se lo pasemos a la función render, el template html deberá contener el siguiente bloque:

```html
<‍form lang="en" id="simple_dynamic_form">
    {% csrf_token %}

        <‍div class="row">
            {% for field in fields %}
            <‍div class="col-sm-12 col-md-4 work-item">
                <‍div class="col-12">
                    <‍div class="ellipsis center d-flex justify-content-center" data-toggle="tooltip">
                        <‍span>{{ field.label | safe }}<‍/span>
                    <‍/div>
                <‍/div>
                <‍div class="col-12">
                    <‍div class="ellipsis center d-flex justify-content-center">
                        {{ field | safe }}
                    <‍/div>
                <‍/div>
            <‍/div>

            {% endfor %}
        <‍/div>
    


    <‍div class="col-12 center mt-2" align="center">
        <‍input id="btnResults" class="btn btn-outline-dark" type="submit" value="Calculate">
    <‍/div>

<‍/form>
```

Nótese que pasamos el formulario a este template bajo el nombre `fields` y que utilizamos los templates de django para recorrer cada uno y renderizarlo. Un aspecto tremendamente importante es que utilizamos el sistema grid de bootstrap para pintar nuestros campos en un cuadrícula. Las clases `col-sm-12 col-md-4` nos indican que en condiciones de tamaño de ventana media o superior cada campo ocupará 4 de las 12 columnas, por lo que tendremos tres campos de por fila, mientras que cuando la pantalla sea pequeña, pasaremos a un campos por fila. En la siguientes figuras puede observarse las diferencias.


![3 cols form](/img/posts/djangoform/3cols.webp)


![1 col form](/img/posts/djangoform/1col.webp)

Nota: Las imágenes corresponden a un formalario dónde también tenía variables categóricas que está representadas con los droplists.

# Código completo

```python

    from django import forms

    class SimpleDynamicForm(forms.Form):
    def __init__(self, *args, **kwargs):
    super(SimpleDynamicForm, self).__init__(*args)
    # Informacion para construir los campos en forma de DataSet
    fields_df: pd.DataFrame = kwargs.get('fields_df')
    initial: dict = kwargs.pop('initial', {})

    for row in fields_df.itertuples():
        # nombre y tipo,
        name_field = row.var
        type_field = row.type
        tooltip_text = row.tooltiptext

        if tooltip_text:
            label_field = f'<‍div data-toggle="tooltip" title="{tooltip_text}">{name_field}'
        else:
            label_field = name_field

        if type_field == 'int64':
            actual_field = forms.IntegerField(label=label_field,
                                            required=True,
                                            min_value=row.min,
                                            max_value=row.max,
                                            initial=int(initial.get(name_field)) if initial.get(
                                                name_field) is not None else None)

        elif type_field == 'float64':
            actual_field = forms.FloatField(label=label_field,
                                            required=True,
                                            min_value=row.min,
                                            max_value=row.max,
                                            initial=initial.get(name_field),
                                            widget=NumberInput(
                                                attrs={'step': "0.01"}))

        # agregar a self.fields
        self.fields[name_field] = actual_field
```