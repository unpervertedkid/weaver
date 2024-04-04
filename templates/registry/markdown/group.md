{%- set file_name = ctx.id | file_name -%}
{{- template.set_file_name("group/" ~ file_name ~ ".md") -}}

# Group `{{ ctx.id }}` ({{ ctx.type }})

## Brief

{{ ctx.brief | trim }}

prefix: {{ ctx.prefix }}

## Attributes

{% for attribute in ctx.attributes %}
### Attribute `{{ attribute.name }}`

{{ attribute.brief }}

{% if attribute.note %}
{{ attribute.note | trim }}
{% endif %}

{%- if attribute.requirement_level == "required" %}
- Requirement Level: Required
{%- elif attribute.requirement_level.conditionally_required %}
- Requirement Level: Conditionally Required - {{ attribute.requirement_level.conditionally_required }}
{%- elif attribute.requirement_level == "recommended" %}
- Requirement Level: Recommended
{%- else %}
- Requirement Level: Optional
{%- endif %}
{% if attribute.tag %}
- Tag: {{ attribute.tag }}
{% endif %}
{%- include "attribute_type.j2" %}
{%- include "examples.j2" -%}
{%- if attribute.sampling_relevant %}
- Sampling relevant: {{ attribute.sampling_relevant }}
{%- endif %}
{%- if attribute.deprecated %}
- Deprecated: {{ attribute.deprecated }}
{%- endif %}
{% if attribute.stability %}
- Stability: {{ attribute.stability | capitalize }}
{% endif %}

{% if ctx.lineage.attributes %}
{% set attr_lineage = ctx.lineage.attributes[attribute.name] %}
{% if attr_lineage %}
Lineage: 
- source group: {{ attr_lineage.source_group }}
- inherited fields: {{ attr_lineage.inherited_fields | join(", ") }}
- locally overridden fields: {{ attr_lineage.locally_overridden_fields | join(", ") }}
{% endif %}
{% endif %}
{% endfor %}