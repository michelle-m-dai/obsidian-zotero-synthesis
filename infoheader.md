---

tags: 
{%- for tag in tags %}
- {{tag.tag}}
{%- endfor %}

---
```ad-note
title: Citation
{{bibliography}}
```

{% persist "mainpoint" %}
{% if isFirstImport %}

```ad-mainpoint
title: punctum saliens
**Takeaways**::
```
{% endif %}
{% endpersist %}

```ad-info
title: Metadata
**Title**:: {{title}}

{% for creator in creators %} {% if creator.name == null %}**{{creator.creatorType | capitalize}}**:: {{creator.lastName}}, {{creator.firstName}}{% endif %}
{% if creator.name %}**{{creator.creatorType | capitalize}}**:: {{creator.name}}{% endif %}{% endfor %}
**Year**:: {{date | format("YYYY")}}
**Citekey**:: @{{citekey}} 
{% if itemType %}**itemType**:: {{itemType}} {% endif %}
{% if itemType == "journalArticle" %}**Journal**:: *{{publicationTitle}}*  {% endif %}
{% if volume %}**Volume**:: {{volume}}  {% endif %}
{% if issue %}**Issue**:: {{issue}}  {% endif %}
{% if itemType == "bookSection" %}**Book**:: {{publicationTitle}}  {% endif %}
{% if publisher %}**Publisher**:: {{publisher}}  {% endif %}
{% if place %}**Location**:: {{place}}  {% endif %}
{% if pages %} **Pages**:: {{pages}}  {% endif %}
{% if DOI %}**DOI**:: {{DOI}}  {% endif %}
{% if ISBN %}**ISBN**:: {{ISBN}}  {% endif %}

{% if hashTags %}
**Tags**:: {% for tag in tags -%}
#{{tag.tag}} {% endfor -%}
{% endif %}

{{extra}}
```

> [!link] Link
> {%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}
> [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}}) {%- endfor -%}.


{%- if abstractNote %}
```ad-abstract
{{abstractNote}}
```
{%- endif -%}

{%- if annotations.length %} 
## Annotations
### Exported: {{exportDate | format("YYYY-MM-DD h:mm a")}}

```ad-note
title: Key
- 🟡 interesting point
- 🟢 literature to read
- 🔵 key conclusion / *ratio decidendi*
- 🟣 author critique of other literature
- 🔴 disagree with author(s)
- 🟠️ highlights after first iteration
- ⚫ methods / references
```

{%- endif %}
{% persist "annotations" %}

{% macro calloutHeader(type, color) %}
{%- if type == "highlight" -%}
[!custom-{{color}}]+ Annotation
{%- endif -%}{%- if type == "text" -%}
Note
{%- endif -%}
{%- endmacro -%}{%- set annots = annotations | filterby("date", "dateafter", lastExportDate) -%}
{%- if annots.length > 0 %}
{% for annot in annots -%}
> {{calloutHeader(annot.type, annot.color)}}
{%- if annot.annotatedText %}
> {{annot.annotatedText | nl2br}}
{%- endif -%}
{%- if annot.imageRelativePath %}
> ![[{{annot.imageRelativePath}}]]
{%- endif %}
{%- if annot.comment %}
> <br> **Comments**:: {{annot.comment}}
{% endif %}

{% endfor -%}
{% endif -%}
{% endpersist %}
