Introduction
============
This project provides one single bash script for managing yaml
files.

Usage:
======
Down load yaml_config file and place in a directory, then source the yaml_config
file into your environment. Load an existing yaml file or simply start setting up
new varilables with the commands provided by the script.  After satisfaction, save
the items from the original yaml and new items into a yaml file.

All commands provided by the script will require a namespace to be specified
as the very first parameter, this is to avoid the conflict when an application
may need to deal with multiple yaml configuration files at the same time.
Each command will take the format like the following::

    function_name namespace key value

Make the functions available::

    source yaml-config

Load an existing yaml file:

    load_yaml 'myyamlfile__' /my_yaml_file.yml

The above command will load a yaml file named my_yaml_file.yml into memory
under namespace 'myyamlfile__' for further processing

Save variables into a yaml file after processing::

    save_yaml 'myyamlfile__' /my_yaml_file.yml

The above command will save all variables in namespace 'myyamlfile__' into
a yaml file named my_yaml_file.yml. The file produced by using this command
will produce a flat key name value yaml file. For example::

    http.host: fasthost.tong.com
    http.port: 80

Save variables into a true yaml file after processing::

    save_normal_yaml 'myyamlfile__' /my_yaml_file.yml

The above command will save all variables in namespace 'myyamlfile__' into
a yaml file named my_yaml)file.yml. The file produced by using this command
will produce a true yaml 2 white space indented yaml file. For example::

    http:
      host: fasthost.tong.com
      port: 80

Set a value::

    set_yaml_value 'myyamlfile__' 'http.hostname' 'fasthost'

The above command will add or set http.hostname value to be 'fasthost'

Get a value::

    get_yaml_value 'myyamlfile__' 'http.host'

The above command will return the value of http.host

Delete a value::

    del_yaml_value 'myyamlfile__' 'http.host'

The above command will remove variable http.host

Other useful commands::

    get_yaml_keys
    get_yaml_values
    del_yaml_values

Command examples:

    del_yaml_values 'myyamlfile__'
    clear all the variables in namespace myyamlfile__

    del_yaml_value 'myyamlfile__' 'http'
    clear all the variables in namespace myyamlfile__ in which the
    variable name start with 'http'

    get_yaml_keys 'myyamlfile__'
    list all the keys in namespace 'myyamlfile__'

Usage example:

    Assume you have a yaml file named servers.yml and it likes this::
        host:
          name: "database_server"
          ip: "192.168.5.23"
          access_port: 80
          
    
    Assume you would like to add the following two new members and
    change access_port to 81 and then save the file, here is how you
    can do this::
    
        source yaml_config
        load_yaml 'example__' servers.yml
        set_yaml_value 'example__' 'host.access_port' 81
        set_yaml_value 'example__' 'zone.owner' 'Fred'
        set_yaml_value 'example__' 'zone.region' 'American'
        save_normal_yaml 'example__' servers.yml
        
    The above commands will produce a new file like the following:
    
        host:
          name: "database_server"
          ip: "192.168.5.23"
          access_port: 81
        zone:
          owner: Fred
          region: American
    

Restrictions:
1. Yaml file must use two spaces for indentation.
2. Comments in the yaml file will be lost.
