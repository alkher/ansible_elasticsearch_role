*************
elasticsearch
*************

Instalación y configuración de `elasticsearch`_.

Basado en el trabajo de `Larry Smith Jr.`_

**********
Requisitos
**********

Sistemas operativos soportados:

	- CentOS 7 

*********
Variables
*********

::

	# LVM
	es_config_lvm: false # Configurar LVM para los nodos de datos, **asegurarse** que los discos son correctos (por defecto false). Poner a true para los *data node*
	es_vgname: elasticsearch-vg # Nombre del VG (se crea si es_config_lvm es true)
	es_disks: '/dev/sdb' # Discos para el VG (por ejemplo: '/dev/sdb,/dev/sdc,/dev/sdd')
	es_lvname: elasticsearch-lv # Nombre del LV (se crea si es_config_lvm es true)
	es_mntp: /var/lib/elasticsearch # Punto de montaje para el volumen
	# ELASTICSEARCH
	es_version: 1.7 # Versión de elasticsearch
	es_fqdn: localhost # IP, FQDN o localhost (usado en el plugin curator)
	es_cluster_name: LA-ELK # Nombre del cluster
	es_replicas: 2  # Número de replicas por shard en el cluster (por defecto 1): [Nº Nodos - 1]
	es_shards: 5  # Número de shards por índice (por defecto 5)
	es_data_node: false  # Verdadero si es un nodo de datos dentro del cluster (por defecto true)
	es_master_node: false  # Verdadero si es un nodo master dentro del cluster (por defecto true)
	es_cluster_setup: false  # Verdadero si es un cluster de elasticsearch (por defecto false)
	es_min_master_nodes: 2  # Número mínimo de nodos master en el cluster para que no se produzca un split brain (por defecto vacío []). Requerido si es_cluster_setup == true [	Nº Nodos/2 + 1]
	es_unicast: true # Verdadero si es un cluster elastic search en modo unicast (por defecto false, multicast)
	es_unicast_hosts: ["elk-es1", "elk-es2", "elk-es3"] # Lista con los nodos del cluster
	es_memory_tuning:  # Ayuda a eliminar condiciones OOM (out of memory)
	  - name: indices.breaker.fielddata.limit
	    set: true # Por defecto false
	    value: 60%  # Por defecto 60%
	  - name: indices.breaker.request.limit
	    set: true # Por defecto false
	    value: 40%  # Por defecto 40%
	  - name: indices.breaker.total.limit
	    set: true # Por defecto false
	    value: 40%  # Por defecto 40%
	  - name: indices.fielddata.cache.size
	    set: true # Por defecto false
	    value: 40%  # Por defecto sin definir
	es_heap_size: 1g # Cantidad de memoria reservada para el heap de elasticsearch (por defecto es 1GB) [50% de la memoria instalada]
	# PLUGINS
	install_head: false # Plugin HEAD (por defecto false)
	install_marvel: false # Plugin MARVEL (por defecto false)
	install_eshq: false # Plugin ESHQ (por defecto false)
	install_bigdesk: false # Plugin BIGDESK (por defecto false)
	install_curator: false # Plugin CURATOR (por defecto false)
	es_curator_max_keep_days: 30  # Número de días que se mantienen los indices (por defecto 360)

************
Dependencias
************

Roles:

	- alkher.java

*******************
Ejemplo de playbook
*******************

::

    - hosts: servidores
      roles:
         - { role: alkher.elasticsearch }

********
Licencia
********

BSD

*****
Autor
*****

	Andoni Alcalde
	- @alkher
	- http://zenway.es
	- alcher [at] zenway [dot] es


.. _elasticsearch: https://www.elastic.co/products/elasticsearch
.. _Larry Smith Jr.: http://everythingshouldbevirtual.com/