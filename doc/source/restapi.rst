..
 Copyright Contributors to the Feilong Project.
 SPDX-License-Identifier: CC-BY-4.0

RESTful APIs
************

This is a reference for the Feilong RESTful API.

Response Data Definition
========================

The following table gives a reference of general response data definition
of each Feilong RESTful API. In case of encountering an error,
those information will be helpful to report bug/issue.

.. restapi_parameters:: parameters.yaml

  - overallRC: ret_overallrc
  - rc: ret_rc
  - rs: ret_rs
  - errmsg: ret_errmsg
  - modID: ret_modID
  - output: ret_output

The following detail API description will only cover ``output`` part as
it's different for each API.
(This document is a beta version now)

Version
=======

Lists version of this API.

Get Feilong version
-------------------

**GET /**

* Request:

  No parameters needed.

* Response code:

  HTTP status code 200 on success.

* Response contents:

  Return the version of the Feilong API.

.. restapi_parameters:: parameters.yaml

  - output : ret_output
  - api_version: api_version_sdk
  - min_version: min_version_sdk
  - max_version: max_version_sdk
  - version: version_sdk

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_version.tpl
   :language: javascript

Token
=====

Management of authentication token.

Create token
------------

**POST /token**

Get a valid token to perform further requests by using an admin token,
which you can think as a combination of username and password.

* Request:

.. restapi_parameters:: parameters.yaml

  - X-Admin-Token: token_admin

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - X-Auth-Token: auth_token

* Response sample:

  Please refer to :ref:`TokenUsage` to get more details.

SMAPI Health
============

Check health of SMAPI layer used by Feilong.

Report health of SMAPI
----------------------

**GET /smapi_health**

Get health status of SMAPI.

* Request:

  None

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: smapi_health_report
  - totalSuccess: totalSuccess
  - totalFail: totalFail
  - lastSuccess: lastSuccess
  - lastFail: lastFail
  - continuousFail: continuousFail
  - healthy: healthy

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_smapi_health.tpl
   :language: javascript

* Note:

  Old API call **GET /smapi-healthy** is deprecated.

Guest(s)
========

Management of z/VM guests and their peripherals.

List guests
-----------

**GET /guests**

List names of all the guests created by Feilong.

* Request:

  None

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: guest_list

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guests_list.tpl
   :language: javascript

Create guest
------------

**POST /guests**

Create a VM in z/VM.

* Request:

.. restapi_parameters:: parameters.yaml

  - guest: guest_dict
  - userid: userid_create
  - vcpus: guest_vcpus
  - memory: guest_memory
  - user_profile: user_profile_guest
  - disk_list: disk_list_guest
  - size: size_disk
  - format: format_disk
  - is_boot_disk: is_boot_disk
  - vdev: add_mdisk_disk_vdev
  - disk_pool: disk_pool_guest
  - max_cpu: guest_max_cpu
  - max_mem: guest_max_mem
  - ipl_from: guest_ipl_from
  - ipl_param: guest_ipl_param
  - ipl_loadparam: guest_ipl_loadparam
  - dedicate_vdevs: guest_dedicate_vdevs
  - loaddev: guest_loaddev
  - account: guest_account
  - comment_list: guest_comment_list


* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_create.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: disk_list_output
  - vdev: vdev_disk
  - size: size_output
  - format: format_disk
  - is_boot_disk: is_boot_disk
  - disk_pool: disk_pool_output

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_disk_output.tpl
   :language: javascript

Get guest minidisks info
------------------------

**GET /guests/{userid}/disks**

List characteristics of all disks of a guest.

* Request:

  None

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - minidisks: minidisks_guest
  - vdev: get_mdisk_disk_vdev
  - rdev: rdev_disk
  - access_type: access_type
  - device_size: size_no_unit
  - device_units: device_units
  - volume_label: volume_label

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_disk_info_output.tpl
   :language: javascript

Guest add disks
---------------

**POST /guests/{userid}/disks**

Add disks to a guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - disk_info: disk_info
  - disk_list: disk_list_guest
  - size: size_disk
  - format: format_disk
  - is_boot_disk: is_boot_disk
  - vdev: add_mdisk_disk_vdev
  - disk_pool: disk_pool_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_add_disks.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: disk_list_output
  - vdev: vdev_disk
  - size: size_output
  - format: format_disk
  - is_boot_disk: is_boot_disk
  - disk_pool: disk_pool_output

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_disk_output.tpl
   :language: javascript

Guest configure disks
---------------------

**PUT /guests/{userid}/disks**

Configure additional disks for a guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - disk_info: disk_info
  - disk_list: additional_disk_guest
  - vdev: disk_vdev
  - format: format_disk_required
  - mntdir: disk_mountpoint

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_config_disks.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No Response

Guest delete disks
------------------

**DELETE /guests/{userid}/disks**

Delete disks from a guest in shutdown state.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - vdev_info: vdev_info
  - vdev_list: disk_vdev_list

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_delete_disks.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No Response

.. note::

  Not support delete disks when guest is active

Attach volume
-------------

**POST /guests/volumes**

Attach a volume to a VM in z/VM.

* Request:

.. restapi_parameters:: parameters.yaml

  - info: volume_info
  - connection: volume_conn
  - assigner_id: assigner_id
  - zvm_fcp: fcp_list
  - fcp_template_id: fcp_template_id
  - target_wwpn: volume_wwpn
  - target_lun: volume_lun
  - os_version: guest_os_version
  - multipath: guest_multipath
  - mount_point: mount_point
  - is_root_volume: root_volume
  - do_rollback: do_rollback

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_volume_attach.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No Response

* Note:

  If multipath is requested, multipath software must be installed on the guest,
  and multipathd service must be running, for this method to work.

Detach volume
-------------

**DELETE /guests/volumes**

Detach a volume from a VM in z/VM.

* Request:

.. restapi_parameters:: parameters.yaml

  - info: volume_info
  - connection: volume_conn
  - assigner_id: assigner_id
  - zvm_fcp: fcp_list
  - target_wwpn: volume_wwpn
  - target_lun: volume_lun
  - os_version: guest_os_version
  - multipath: guest_multipath
  - mount_point: mount_point
  - is_root_volume: root_volume
  - update_connections_only: update_connections_only
  - do_rollback: do_rollback

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_volume_detach.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No Response

Get guests stats including CPU and memory
-----------------------------------------

**GET /guests/stats**

Get guests CPU and memory information.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: userid_list_guest

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: stats_guest
  - guest_cpus: guest_cpus
  - used_cpu_time_us: used_cpu_time_us
  - elapsed_cpu_time_us: elapsed_cpu_time_us
  - min_cpu_count: min_cpu_count
  - max_cpu_limit: max_cpu_limit
  - samples_cpu_in_use: samples_cpu_in_use
  - samples_cpu_delay: samples_cpu_delay
  - used_mem_kb: used_mem_kb
  - max_mem_kb: max_mem_kb
  - min_mem_kb: min_mem_kb
  - shared_mem_kb: shared_mem_kb
  - total_memory: total_memory
  - available_memory: available_memory
  - free_memory: free_memory

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guests_get_stats.tpl
   :language: javascript

Get guests interface stats
--------------------------

**GET /guests/interfacestats**

Get guests network interface statistics.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: userid_list_guest

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: guest_vnics
  - vswitch_name: vswitch_name_opt
  - nic_vdev: nic_interface
  - nic_fr_rx: nic_fr_rx
  - nic_fr_tx: nic_fr_tx
  - nic_fr_rx_dsc: nic_fr_rx_dsc
  - nic_fr_tx_dsc: nic_fr_tx_dsc
  - nic_fr_rx_err: nic_fr_rx_err
  - nic_fr_tx_err: nic_fr_tx_err
  - nic_rx: nic_rx
  - nic_tx: nic_tx

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guests_get_interface_stats.tpl
   :language: javascript

Get guests NIC info
-------------------

**GET /guests/nics**

Get guests NIC information, including userid, NIC number, vswitch, NIC id and comments.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid_opt
  - nic_id: nic_id_opt
  - vswitch: vswitch_name_opt

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: guests_nic_info
  - userid: nic_userid
  - interface: nic_interface
  - switch: vswitch_name_body
  - port: nic_port
  - comments: nic_comments

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guests_get_nic_info.tpl
   :language: javascript

Show guest definition
---------------------

**GET /guests/{userid}**

Display the user direct for the given userid.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: user_direct_guest

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get.tpl
   :language: javascript

Delete guest
------------

**DELETE /guests/{userid}**

Delete a guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No Response


Get guest power state from hypervisor
-------------------------------------

**GET /guests/{userid}/power_state_real**

Get power state of the guest from hypervisor directly,
no matter the guest is in zcc database or not.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: power_status_guest

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get_power_state_real.tpl
   :language: javascript


Get guest info
--------------

**GET /guests/{userid}/info**

Get running information of guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: guest_info
  - max_mem_kb: guest_memory_kb_max
  - num_cpu: num_cpu_guest
  - cpu_time_us: cpu_time_us_guest
  - power_state: power_status_guest
  - mem_kb: guest_memory_kb
  - online_cpu_num: online_cpu_num_guest
  - os_distro: os_distro_guest
  - kernel_info: kernel_info_guest

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get_info.tpl
   :language: javascript


Get guest user direct
---------------------

**GET /guests/{userid}/user_direct**

Get the user directory info of the given userid from hypervisor.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: user_direct_guest

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get_user_direct.tpl
   :language: javascript


Get guest adapters info
-----------------------

**GET /guests/{userid}/adapters**

Get adapters information of running guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: adapters_info_output
  - lan_owner: guest_userid
  - lan_name: vswitch_name
  - adapter_address: vdev_number
  - adapter_status: adapter_status
  - mac_address: mac_address
  - mac_ip_address: ip_address
  - mac_ip_version: ip_version

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get_adapters_info.tpl
   :language: javascript

Create guest NIC
----------------

**POST /guests/{userid}/nic**

Create a virtual NIC on giving guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - nic: nic_set_info
  - vdev: vdev_number_orNone
  - nic_id: nic_identifier
  - mac_addr: mac_address
  - active: active_flag

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_create_nic.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Create network interface
------------------------

**POST /guests/{userid}/interface**

Create one or more network interfaces on giving guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - interface: network_interface_info
  - os_version: guest_os_version
  - guest_networks: guest_networks_list
  - active: active_flag

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_create_network_interface.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Delete network interface
------------------------

**DELETE /guests/{userid}/interface**

Delete a network interface on given guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - interface: network_interface_info
  - os_version: guest_os_version
  - vdev: nic_interface
  - active: active_flag

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_delete_network_interface.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Start guest
-----------

Start a guest.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_start_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_start_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Stop guest
----------

Stop a guest.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_stop_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_stop_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Softstop guest
--------------

Stop a guest gracefully, it will first shut down the OS on the VM, then stop the VM.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_softstop_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_softstop_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Pause guest
-----------

Pause a guest.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_pause_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_pause_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Unpause guest
-------------

Unpause a guest.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_unpause_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_unpause_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Reboot guest
------------

Reboot a guest, this will use 'reboot' command on the
given guest.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_reboot_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_reboot_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Reset guest
-----------

Reset a guest, this will first gracefully logoff the guest from
z/VM it is running on, then log on the guest and IPL.

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_reset_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_reset_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Get guest console output
------------------------

**POST /guests/{userid}/action**

Get console output of guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_get_console_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get_console_output_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  - output: console_output

.. note::

   In order to retrieve the console log from guest vm, you must add user direct
   statment "COMMAND SP CONS * START" to the profile that used to deploy guest
   vm, otherwise no console log collected for the guest vm.

Live migration of guest
-----------------------

**POST /guests/{userid}/action**

Live migrate guest in z/VM SSI cluster.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_live_migrate_guest
  - dest_zcc_userid: dest_zcc_userid
  - destination: lgr_destination
  - parms: lgr_parms
  - lgr_action: lgr_action

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_live_migrate.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Guest register
--------------

**POST /guests/{userid}/action**

Register guest to be managed by Feilong.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_register_guest
  - meta: guest_register_meta
  - net_set: guest_register_net_set
  - port: guest_register_port_macs

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_register.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Guest deregister
----------------

**POST /guests/{userid}/action**

Deregister guest to be managed by Feilong.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_deregister_guest

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_deregister.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Live resize CPUs of guest
-------------------------

**POST /guests/{userid}/action**

Live resize CPUs of guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_live_resize_cpus_of_guest
  - cpu_cnt: cpu_cnt

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_live_resize_cpus_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. note::

   - Currently only increasing CPU count is supported, decreasing is not supported.
   - The guest to be live resized must be active and managed by Feilong.
   - If live resize finished successfully, both the active CPU number and the number of
     defined CPUs in user directory would be updated to the requested so that the CPU
     count would persist even after the guest is restarted.
   - If requested CPU count is smaller than the current active CPU count, then the API
     would return error immediately since lively decrease CPU count is not supported.
   - If requested CPU count is larger than the defined CPU number in user directory,
     this API would increase the CPU number in user directory to requested count.
     Otherwise, this API would decrease the CPU number in user directory to the requested
     count.
   - To live resize a guest, the guest must have maximum CPU count defined in user
     directory entry with "MACHINE ESA xx" where 'xx' is the maximum CPU count. The
     resize CPU count can't exceed the maximum CPU count.
   - For guests created by Feilong after version 1.2.0, the maximum CPU
     count is defined when the guest is created. The maximum CPU count is set by the configuration
     "user_default_max_cpu" in [zvm] section and can be overriden by the parameter "max_cpu" when
     creating the guest. e.g, the following configuration would define the default maximum CPU count
     as 64.

     .. code-block:: text

         [zvm]
         user_default_max_cpu=64

Resize CPUs of guest
--------------------

**POST /guests/{userid}/action**

Resize CPUs of guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_resize_cpus_of_guest
  - cpu_cnt: cpu_cnt

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_resize_cpus_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. note::

   - Both increasing and decreasing CPU count are supported.
   - The target guest can be in either 'on' or 'off' status, the definition change would
     take into effects after logoff and re-logon (reset).

Live resize memory of guest
---------------------------

**POST /guests/{userid}/action**

Live resize memory of guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_live_resize_mem_of_guest
  - size: mem_size

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_live_resize_mem_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. note::

   - Currently only increasing memory size is supported, decreasing is not supported.
   - The guest to be live resized must be active and managed by Feilong.
   - If live resize finished successfully, both the active memory and the initial memory
     defined in user directory would be updated to the requested size so that the change
     would persist even after the guest is restarted.
   - If requested memory size is smaller than the current active memory, then the API
     would return error immediately since lively decrease memory size is not supported.
   - If requested memory size is larger than the defined initial memory in user directory,
     this API would increase the initial memory defined in user directory to requested size.
     Otherwise, this API would decrease the memory in user directory to the requested
     size.
   - The resize memory size can't exceed the maximum memory defined in user directory.
   - The maximum memory size is defined when the guest is created. It is set by the configuration
     "user_default_max_memory" in [zvm] section and can be overriden by the parameter "max_mem" when
     creating the guest. For example, the following configuration would define the default maximum
     memory size as 64G.

     .. code-block:: text

         [zvm]
         user_default_max_memory=64g

Resize memory of guest
----------------------

**POST /guests/{userid}/action**

Resize memory of guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_resize_mem_of_guest
  - size: mem_size

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_resize_mem_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. note::

   - Both increasing and decreasing memory size are supported.
   - The target guest can be in either 'on' or 'off' status, the definition change would
     take into effects after logoff and re-logon (reset).

Deploy guest
------------

**POST /guests/{userid}/action**

After guest is created, deploy image onto the guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_deploy_guest
  - image: image_name
  - transportfiles: transportfiles
  - remotehost: remotehost_transportfiles
  - vdev: deploy_vdev
  - hostname: deploy_hostname
  - skipdiskcopy: deploy_skipdiskcopy

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_deploy_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Capture guest
-------------

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_capture_guest
  - image: image_name
  - capture_type: capture_type
  - compress_level: compress_level

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_capture_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Grow root volume of guest
-------------------------

**POST /guests/{userid}/action**

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - action: action_grow_root_volume_guest
  - os_version: guest_os_version

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_grow_root_volume_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

Get guest power state
---------------------

**GET /guests/{userid}/power_state**

Get power state of the guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid

* Response code:

  HTTP status code 200 on success.
  HTTP status code 404 if guest is not in zcc database
  or it is in zcc database but its user directory does not exist.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: power_status_guest

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_get_power_state.tpl
   :language: javascript

Update guest NIC
----------------

**PUT /guests/{userid}/nic/{vdev}**

Couple or uncouple NIC with vswitch on the guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - vdev: vdev_guest
  - info: couple_info
  - couple: couple_action
  - active: active_flag
  - vswitch: vswitch_name_body_opt
  - vlan_id: vlan_id

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_guest_couple_uncouple_nic.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Delete guest NIC
----------------

**DELETE /guests/{userid}/nic/{vdev}**

Delete a NIC on the guest.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - vdev: vdev_guest
  - active: active_flag

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Host
====

Get guests list and information from the host (hypervisor) they are running on.

Get guests list
---------------

**GET /host/guests**

List names of all the guests on the host.

* Request:

  None

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: guest_list

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_get_guest_list.tpl
   :language: javascript

Get info from host (hypervisor) running on.

Get host info
-------------

**GET /host**

Get host information.

* Request:

  No parameters needed.

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: host_info

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_info.tpl
   :language: javascript

Get host disk pool info
-----------------------

**GET /host/diskpool**

Get disk pool information(or the free space among all volumes in the disk pool) on the host.

* Request:

.. restapi_parameters:: parameters.yaml

  - poolname: disk_pool
  - details: details

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - disk_available: disk_info_available
  - disk_total: disk_info_total
  - disk_used: disk_info_used

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_disk_info.tpl
   :language: javascript
.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_disk_details_info.tpl
   :language: javascript

Get host disk pool volume names
-------------------------------

**GET /host/diskpool_volumes**

Get volume list of the diskpool on the host.

* Request:

.. restapi_parameters:: parameters.yaml

  - poolname: disk_pool

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - diskpool_volumes: diskpool_volumes

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_disk_volumes.tpl
   :language: javascript

Get host volume info
--------------------

**GET /host/volume**

Get the volume info on the host.

* Request:

.. restapi_parameters:: parameters.yaml

  - volumename: volume

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - volume_type: volume_type
  - volume_size: volume_size

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_volume.tpl
   :language: javascript

Get host SSI cluster info
-------------------------

**GET /host/ssi**

Get the SSI(Single System Image) cluster information of the host.

* Request:

  No parameters needed.

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: host_ssi_info

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_host_ssi_info.tpl
   :language: javascript

Image(s)
========

Lists, creates, shows details for, updates, and deletes images.

List images
-----------

**GET /images**

Get the list of image info in image repository.

* Request:

.. restapi_parameters:: parameters.yaml

  - imagename: imagename

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: image_info
  - imagename: image_name
  - imageosdistro: guest_os_version
  - md5sum: image_md5sum
  - disk_size_units: root_disk_size_image
  - image_size_in_bytes: physical_disk_size_image
  - type: image_type
  - comments: image_comments
  - last_access_time: last_access_time

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_image_query.tpl
   :language: javascript

Create image
------------

**POST /images**

Import an image into image repository

* Request:

.. restapi_parameters:: parameters.yaml

  - image: image_dict
  - image_name: image_name
  - url: image_import_url
  - image_meta: image_metadata
  - remote_host: remotehost_image_import

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_image_create_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Export image
------------

**PUT /images/{name}**

Export the image to the specified location.

* Request:

.. restapi_parameters:: parameters.yaml

  - name: image_name_path
  - location: image_export_location
  - dest_url: image_export_url
  - remote_host: remotehost_image_export

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_image_export_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: export_image_dict

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_image_export_res.tpl
   :language: javascript

Get root disk size of image
---------------------------

**GET /images/{name}/root_disk_size**

Get the root disk size of the image.

* Request:

.. restapi_parameters:: parameters.yaml

  - name: image_name_path

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: root_disk_size_image

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_image_get_root_disk_size.tpl
   :language: javascript

Delete image
------------

**DELETE /images/{name}**

Delete an image.

* Request:

.. restapi_parameters:: parameters.yaml

  - name: image_name_path

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

VSwitch(es)
===========

Management of virtual switches.

Create vswitch
--------------

**POST /vswitches**

Create a new vswitch.

* Request:

.. restapi_parameters:: parameters.yaml

  - name: vswitch_name_body
  - rdev: rdev_vswitch
  - controller: controller_vswitch
  - connection: connection_vswitch
  - network_type: network_type_vswitch
  - router: router_vswitch
  - vid: vid_vswitch
  - port_type: port_type_vswitch
  - gvrp: gvrp_vswitch
  - queue_mem: queue_mem_vswitch
  - native_vid: native_vid_vswitch
  - persist: persist_option_vswitch

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_vswitch_create.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

List vswitches
--------------

**GET /vswitches**

Get the list of vswitch name on the host

* Request:

  No parameter needed.

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: vswitch_list

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_vswitch_get.tpl
   :language: javascript

Get vswitch details
-------------------

**GET /vswitches/{name}**

Get the details of a vswitch

* Request:

.. restapi_parameters:: parameters.yaml

  - name: vswitch_name

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: vswitch_details

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_vswitch_query.tpl
   :language: javascript

Grant user to vswitch
---------------------

**PUT /vswitches/{name}**

Grant an user to access vswitch

* Request:

.. restapi_parameters:: parameters.yaml

  - name: vswitch_name
  - vswitch: vswitch_info
  - grant_userid: grant_userid

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_vswitch_update_req.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Revoke user from vswitch
------------------------

**PUT /vswitches/{name}**

Revoke the user access from vswitch

* Request:

.. restapi_parameters:: parameters.yaml

  - name: vswitch_name
  - vswitch: vswitch_info
  - revoke_userid: grant_userid

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_vswitch_update_revoke.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Set user VLAN id to vswitch
---------------------------

**PUT /vswitches/{name}**

Set VLAN id for user when connecting to the vswitch

* Request:

.. restapi_parameters:: parameters.yaml

  - name: vswitch_name
  - vswitch: vswitch_info
  - user_vlan_id: user_vlan_id
  - userid: grant_userid
  - vlanid: vlan_id

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_vswitch_set_vlan.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Delete vswitch
--------------

**DELETE /vswitches/{name}**

Delete a vswitch by using given name.

* Request:

.. restapi_parameters:: parameters.yaml

  - name: vswitch_name

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Get vswitch info
----------------

**GET /vswitch**

Get vswitch information based on port id.
Returns one entry per connected NIC.

* Request:

.. restapi_parameters:: parameters.yaml

  - portid: port_id

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

   - userid: nic_userid
   - interface: nic_interface
   - switch: vswitch_name_body
   - port: port_id
   - comments: nic_comments

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_get_switch_record.tpl
   :language: javascript

Volume(s)
=========

Handling of volumes and FCP devices.

Refresh volume bootmap info
---------------------------

**PUT /volumes/volume_refresh_bootmap**

Refresh a volume's bootmap info.

* Request:

.. restapi_parameters:: parameters.yaml

  - fcpchannel: fcp_list
  - wwpn: wwpn_list
  - lun: lun
  - transportfiles: transportfiles
  - guest_networks: guest_networks

* Request sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_refresh_bootmap.tpl
   :language: javascript

* Response code:

  HTTP status code 200 on success.

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_refresh_bootmap_response.tpl
   :language: javascript

Get volume connector
--------------------

**GET /volumes/conn/{userid}**

Get volume connector for a given userid.

* Request:

.. restapi_parameters:: parameters.yaml

  - userid: guest_userid
  - reserve: fcp_reserve
  - fcp_template_id: fcp_template_id
  - storage_provider: storage_provider
  - pchid_info: pchid_info

* Response code:

  HTTP status code 200 on success.

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_get_volume_connector.tpl
   :language: javascript

Create FCP template
-------------------

**POST /volumes/fcptemplates**

Create a FCP multipath template.

* Request:

.. restapi_parameters:: parameters.yaml

   - name: fcp_template_name
   - description: fcp_template_description
   - fcp_devices: fcp_template_fcp_devices
   - host_default: fcp_template_host_default
   - storage_providers: fcp_template_storage_providers
   - min_fcp_paths_count: fcp_template_min_fcp_paths_count

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

   - fcp_template: fcp_template
   - id: fcp_template_id
   - name: fcp_template_name
   - description: fcp_template_description_output
   - host_default: fcp_template_host_default_output
   - storage_providers: fcp_template_storage_providers_output
   - min_fcp_paths_count: fcp_template_min_fcp_paths_count_output

Delete FCP template
-------------------

**DELETE /volumes/fcptemplates/{template_id}**

Delete a FCP multipath template.

* Request:

.. restapi_parameters:: parameters.yaml

   - template_id: fcp_template_id_path

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Get FCP templates
-----------------

**GET /volumes/fcptemplates**

Get a list of FCP templates.

* Request:

.. restapi_parameters:: parameters.yaml

   - template_id_list: template_id_list
   - assigner_id: userid_in_params
   - default_sp_list: default_sp_list
   - host_default: host_default

* Request sample:

.. code-block:: text

   https://<feilong_fqdn>/volumes/fcptemplates?template_id_list=[
     "45c1d1be-a437-11ef-be87-e1b40fa110c4","4982739c-a457-11ef-be87-e1b40fa110c4"]

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

   - name: fcp_template_name
   - id: fcp_template_id
   - description: fcp_template_description_output
   - host_default: fcp_template_host_default_output
   - sp_default: fcp_template_sp_default
   - min_fcp_paths_count: fcp_template_min_fcp_paths_count_output
   - cpc_sn: cpc_sn
   - cpc_name: cpc_name
   - lpar: lpar
   - hypervisor_hostname: hypervisor_hostname
   - pchids: pchids

Edit FCP template
-----------------

**PUT /volumes/fcptemplates/{template_id}**

Change some values in an existing FCP template.

* Request:

.. restapi_parameters:: parameters.yaml

   - template_id: fcp_template_id_path
   - name: fcp_template_name_optional
   - description: fcp_template_description
   - fcp_devices: fcp_template_fcp_devices
   - host_default: fcp_template_host_default
   - default_sp_list: fcp_template_default_sp_list
   - min_fcp_paths_count: fcp_template_min_fcp_paths_count

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Get FCP templates details
-------------------------

**GET /volumes/fcptemplates/detail**

Get more information about FCP templates, including raw data and/or statistics.

* Request:

.. restapi_parameters:: parameters.yaml

   - template_id_list: template_id_list
   - raw: get_raw_data
   - statistics: get_statistics_data
   - sync_with_zvm: sync_with_zvm

* Request sample:

.. code-block:: text

   https://<feilong_fqdn>/volumes/fcptemplates?template_id_list=[
     "45c1d1be-a437-11ef-be87-e1b40fa110c4","4982739c-a457-11ef-be87-e1b40fa110c4"]&
     statistics=true

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

   - id: fcp_template_id
   - name: fcp_template_name
   - description: fcp_template_description_output
   - host_default: fcp_template_host_default_output
   - storage_providers: fcp_template_storage_providers_output
   - min_fcp_paths_count: fcp_template_min_fcp_paths_count_output
   - raw: fcp_details_raw
   - statistics: fcp_details_statistics
   - cpc_sn: cpc_sn
   - cpc_name: cpc_name
   - lpar: lpar
   - hypervisor_hostname: hypervisor_hostname
   - pchids: pchids

Get FCP usage
-------------

**GET /volumes/fcp/{fcp_id}**

Get the FCP usage in database for z/VM.

* Request:

.. restapi_parameters:: parameters.yaml

  - fcp_id: volume_fcp

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: fcp_usage

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_get_fcp_usage.tpl
   :language: javascript

Set FCP usage
-------------

**PUT /volumes/fcp/{fcp_id}**

Set the FCP usage in database for z/VM.

* Request:

.. restapi_parameters:: parameters.yaml

  - fcp_id: volume_fcp
  - userid: fcp_userid
  - reserved: fcp_reserved
  - connections: fcp_connections
  - fcp_template_id: fcp_template_id

* Response code:

  HTTP status code 200 on success.

* Response contents:

  No response.

Files
=====

Imports and exports raw file data.

These operations may be restricted to Feilong administrators.

Import file
-----------

**PUT /files**

Import binary file data to Feilong. Internal use only. Please set
the Content-Type of the request header to application/octet-stream. The body
contains the binary data.

* Request:

.. restapi_parameters:: parameters.yaml

  - Content-type: file_request_header

* Example call:

  curl http://192.168.99.3:8888/files -i -X PUT -H "X-Auth-Token: $token" -H
   "Content-Type: application/octet-stream" --data-binary @/root/testfile

* Response code:

  HTTP status code 200 on success.

* Response contents:

.. restapi_parameters:: parameters.yaml

  - output: file_import_output
  - dest_url: file_import_dest_url
  - filesize_in_bytes: imported_file_size
  - md5sum: file_md5sum

* Response sample:

.. literalinclude:: ../../zvmsdk/tests/fvt/api_templates/test_file_import_res.tpl
   :language: javascript


Export file
-----------

**POST /files**

Export file from Feilong, internal use only.

* Request:

.. restapi_parameters:: parameters.yaml

  - source_file: export_source_file

* Response code:

  HTTP status code 200 on success.

The response body contains the raw binary data that represents the actual file.
The Content-Type header contains the application/octet-stream value.
