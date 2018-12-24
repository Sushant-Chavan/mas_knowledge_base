# ``mas_knowledge_base``

# Table of Contents
1. [Summary](#summary)
2. [Package organisation](#package-organisation)
3. [Dependencies](#dependencies)
4. [Short API description](#short-api-description)
    1. [mas_knowledge_utils](#mas_knowledge_utils)
        1. [ontology_query_interface](#ontology_query_interface)
    2. [mas_knowledge_base](#mas_knowledge_base)
        1. [knowledge_base_interface](#knowledge_base_interface)
        2. [domestic_kb_interface](#domestic_kb_interface)

## Summary

`mas_knowledge_base` is a collection of packages for equipping robots with capabilities for using knowledge in their operation. We particularly consider two types of knowledge robots may have:
1. *Ontological knowledge* about concepts in the world and the connections between those; this knowledge is mostly static and rarely changes
2. *Online knowledge* about the world that gets updated as a robot operates in the environment. Online knowledge can further be divided into two groups (this division of knowledge goes in line with how [ROSPlan](http://kcl-planning.github.io/ROSPlan/documentation/) defines its knowledge base):
    1. Symbolic knowledge about the world (e.g. a bottle may be on a table or a spoon might be in a kitchen drawer)
    2. Complete information about objects in the world (e.g. the pose and the point cloud of an object)

The above division of knowledge is used as a building block for the two Python packages that are defined in this repository:
* `mas_knowledge_utils`: A ROS-independent package that implements functionalities for ontology interaction. The packages assumes that ontologies are defined using [OWL](https://www.w3.org/OWL/) and provides an interface for interacting with those.
* `mas_knowledge_base`: A ROS-dependent package that implements functionalities for interacting with online knowledge. The dependency on ROS arises because the package makes use of MongoDB store and the ROSPlan knowledge base.

In addition to these packages, the repository includes one ontology - a so-called apartment ontology - that defines classes of objects encountered in a common domestic environment.

## Package organisation

The package has the following structure:
```
mas_knowledge_base
|    package.xml
|    CMakeLists.txt
|    setup.py
|    README.md
|    LICENSE
|____common
|    |____mas_knowledge_utils
|    |    |    __init__.py
|    |    |____ontology_query_interface.py
|    |
|    |____ontology
|    |    |____apartment.owl
|    |
|____ros
     |____src
          |____mas_knowledge_base
               |    __init__.py
               |    knowledge_base_interface.py
               |____domestic_kb_interface.py


```

## Dependencies

* ``rdflib`` (for the ontology query interface)
* ``mongodb_store`` (for the knowledge base interface)
* ``rosplan_knowledge_msgs`` (for the knowledge base interface)

## Short API description

### mas_knowledge_utils

#### ontology_query_interface

The ontology query interface exposes the following methods:
* `is_instance_of`: Returns True if an object is an instance of a given class and False otherwise
* `get_instances_of`: Returns a list of all instances belonging to a given class
* `get_subclasses_of`: Returns a list of all subclasses of a given class
* `get_parent_classes_of`: Returns a list of all ancestor classes of a given class

### mas_knowledge_base

#### knowledge_base_interface

The knowledge base interface exposes methods for interacting both with the ROSPlan symbolic knowledge base and with MongoDB store. The following methods are exposed for interacting with the symbolic knowledge base:
* `get_all_attributes`: Returns a list of all instances of a given predicate
* `update_kb`: Inserts facts into and removes facts from the knowledge base
* `insert_facts`: Inserts a list of facts into the knowledge base
* `remove_facts`: Removes a list of facts from the knowledge base

The following methods are exposed for interacting with MongoDB store (most of which are just wrappers around the built-in MongoDB store methods):
* `insert_objects`: Inserts a list of named objects into the knowledge base
* `update_objects`: Updates a list of named objects in the knowledge base
* `remove_objects`: Removes a list of named objects from the knowledge base
* `get_objects`: Retrieves a list of named objects of a given type from the knowledge base
* `insert_obj_instance`: Inserts a named object instance into the knowledge base
* `update_obj_instance`: Updates a named object instance in the knowledge base
* `remove_obj_instance`: Removes a named object instance with a given type from the knowledge base
* `get_obj_instance`: Retrieves a named object of a given type from the knowledge base

#### domestic_kb_interface

The domestic knowledge base interface exposes methods common for domestic applications:
* `get_surface_object_names`: Returns a list of names of objects that are on a given surface
