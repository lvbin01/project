==========================================
SandStone iSCSI driver
==========================================

SandStone USP volume can be used for virtual machines (VMs) in the
OpenStack Block Storage driver that supports iSCSI  protocols

##System requirements##

| Cluster | version |
| ----------| --------------|
| SandStone USP | 3.2.2+    |

To use the SandStone driver, the following are required:

- Network connectivity between the OpenStack host and the SandStone USP management
  interfaces

- HTTPS or HTTP must be enabled on the array

When creating a volume from image, add the following configuration keys in the ``[DEFAULT]``
configuration group of the ``/etc/cinder/cinder.conf`` file:

##Driver options

   The following table contains the configuration options supported by the
SandStone driver.

      [DEFAULT]
      enabled_backends = sds-iscsi

      [sds-iscsi]
      volume_driver = cinder.volume.drivers.sandstone.sds_driver.SdsISCSIDriver
      volume_backend_name = sds-iscsi
      san_ip = 10.10.16.21
      san_login = admin
      san_password = admin
      default_sandstone_target_ips = 10.10.16.21,10.10.16.22,10.10.16.23
      chap_username = 123456789123
      chap_password = 1234567891234
      sandstone_pool = vms
      initiator_assign_sandstone_target_ip = {"iqn.1993-08.org.debian:01:3a9cd5c484a": "10.10.16.21"}

###Replication parameters###

| Parameter  | Description |
| ----------| --------------|
| volume_driver | indicates the loaded driver|
| volume_backend_name | indicates the name of the backend|
| san_ip | IP addresses of the management interfaces of a storage system|
| san_login | Storage system user name           |
| san_password | Storage system password           |
| default_sandstone_target_ips | Default IP address of the iSCSI target port that is provided for compute nodes.          |
| chap_username |  CHAP authentication username         |
| chap_password |  CHAP authentication password         |
| sandstone_pool |  SandStone storage pool resource name         |
| initiator_assign_sandstone_target_ip |  Initiator assign target with assign ip         |



#. After modifying the ``cinder.conf`` file, restart the ``cinder-volume``
   service.

#. Create and use volume types.

   **Create and use sds-iscsi volume types**

   .. code-block:: console

      $ openstack volume type create sdas
      $ openstack volume type set --property sdas=True sdas

   **Create and use replication volume types**

   .. code-block:: console

      $ openstack volume type create replication
      $ openstack volume type set --property replication_enabled=True replication


