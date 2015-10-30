# ansible_oracle_cloud_modules

## DOCUMENTATION
<pre><code>
module: oracle_cloud_dbpaas
short_description: Manage Oracle Instance Services in the Oracle Cloud
description:
    - Best way to use this module in task playbooks is to delegate_to: 127.0.0.1
version_added: "0.1.0"
options:
    identityDomain:
        description:
            - Name of the identity domain housing the Database as a Service subscription or trial to which the request applies.
        required: true
        default:     
    user:
        description:
            - 
        required: true
        default:
    password:
        description:
            - 
        required: true
        default:    
    state:
        description:
            - The intended state of the Oracle Cloud Instance
                - present = Create New Cloud Service
                - absent = Delete Existen Cloud Service
                - start = Start Existen Cloud Service VM
                - stop  = Stop Existen Cloud Service VM
                - restart = Restart Existen Cloud Service VM
                - scale = Scale Up or Down Cloud Service VM
        required: true
        choices=["present","absent","start", "stop", "restart","scale"]
    serviceName:
        description:
            - A string containing the name for service instance.
        required: true
        default:
    level:
        description:
            - A string containing the service level for the service instance:
                - PAAS: The Oracle Database Cloud Service service level
                - BASIC: The Oracle Database Cloud Service - Virtual Image service level
        required: false
        default: PAAS
        choices=["PAAS","BASIC"]
    subscriptionType:
        description:
            - A string containing the billing frequency for the service instance; either MONTHLY or HOURLY.
        required: false
        default:
    version:
        description:
            - A string containing the Oracle Database version for the service instance:
                - 12.1.0.2
                - 11.2.0.4
        required: false
        default: 12.1.0.2
        choices=["12.1.0.2","11.2.0.4"]
    edition:
        description:
            - A string containing the database edition for the service instance:
                - SE: Standard Edition
                - EE: Enterprise Edition
                - EE_HPÐEnterprise Edition: High Performance
                - EE_EPÐEnterprise Edition: Extreme Performance
        required: false
        default: "EE"
        choices=["SE","EE","EE_HP","EE_EP"]
    description:
        description:
            - A string containing a description of the service instance, if desired.
        required: false
        default:
    shape:
        description:
            - A string containing the Oracle Compute Cloud shape to scale the service instance up to.
        required: false
        default: oc3
        choices=["oc3","oc4","oc5","oc6","oc7","oc1m","oc2m","oc3m","oc4m","oc5m"]
    vmPublicKey:
        description:
            - A string containing the fully qualified name of an SSH public key already uploaded to Oracle Compute Cloud service. This string has the form:
            - Either this parameter or the vmPublicKey parameter must be provided, but not both of them.
        required: false
        default:
    vmPublicKeyText:
        description:
            - A string containing the text of an SSH public key. This key is added to Oracle Compute Cloud Service as part of the instance creation operation.
            - Either this parameter or the vmPublicKey parameter must be provided, but not both of them.
        required: false
        default:
    usableStorage:
        description:
            - A string containing the number of GB of usable data storage for the Oracle Database server .
        required: false
        default:
    adminPassword:
        description:
            - A string containing the administrator password for the service instance.
        required: false
        default:
    sid:
        description:
            - A string containing the SID for the database instance.
        required: false
        default:
    pdbName:
        description:
            - A string containing the name for the default PDB (pluggable database).
            - Include this parameter only if the level is "PAAS", the version is "12.1.0.2" and the edition is EE, EE_HP or EE_EP.
        required: false
        default:    
    backupDestination:
        description:
            - A string containing the backup configuration for the service instance:
                - NONE: Configure no backups.
                - DISK: Configure backups to local storage on the service instance.
                - BOTH: Configure backups to local storage on the service instance and to an Oracle Storage Cloud container.
            - Include this parameter only if the level is "PAAS".
        required: false
        default: NONE
        choices=["NONE","DISK","BOTH"]
    cloudStorageContainer:
        description:
            - A string containing the Oracle Storage Cloud container for backups. This string has the form: instance-id_domain\/container
        required: false
        default:
    cloudStorageUser:
        description:
            - The user name of an Oracle Cloud user with read/write access to the specified
        required: false
        default:
    cloudStoragePwd:
        description:
            - A string containing the password of the specified cloudStorageUser.
        required: false
        default:

    hostname:
        description:
            - The Oracle database host
        required: false
        default: localhost

 </pre></code>   
## notes:
<pre><code>
    - required=True needs to be installed
requirements: [ "request" ]
author: Björn Ahl, bjorn.ahl@gmail.com, @TwittAhl
</pre></code>

## EXAMPLES
### Stop VM in Oracle Cloud
<pre><code>
oracle_cloud_dbpaas: identityDomain: domain1 user: oracle_cloud_admin_user password: oracle_cloud_admin_password serviceName: Oracle Cloud Service Name state: stop
</pre></code>
### start VM in Oracle Cloud
<pre><code>
oracle_cloud_dbpaas: identityDomain: domain1 user: oracle_cloud_admin_user password: oracle_cloud_admin_password serviceName: Oracle Cloud Service Name state: start
</pre></code>
### restart VM in Oracle Cloud
<pre><code>
oracle_cloud_dbpaas: identityDomain: domain1 user: oracle_cloud_admin_user password: oracle_cloud_admin_password serviceName: Oracle Cloud Service Name state: restart
</pre></code>
### Delete Cloud Service in Oracle Cloud (Service that you want to delete need to be stopped state)
<pre><code>
oracle_cloud_dbpaas: identityDomain: domain1 user: oracle_cloud_admin_user password: oracle_cloud_admin_password serviceName: Oracle Cloud Service Name state: absent
</pre></code>
### Scale Up/Down Cloud Service
<pre><code>
oracle_cloud_dbpaas: identityDomain: domain1 user: oracle_cloud_admin_user password: oracle_cloud_admin_password serviceName: Oracle Cloud Service Name state: scale shape: oc5/oc6/oc1m/oc2m/oc3m..
</pre></code>
### Create new Oracle Cloud Service
<pre><code>
oracle_cloud_dbpaas: identityDomain: domain1 user: oracle_cloud_admin_user password: oracle_cloud_admin_password serviceName: Oracle Cloud Service Name state: present serviceName: "vm1" level: PAAS subscriptionType: MONTHLY version: "12.1.0.2" edition: EE description: test01 shape: oc3 vmPublicKey: "/Compute-domain/user@test.se/host"  usableStorage: "30" adminPassword: "Pa55_word" sid: "cdb1"pdbName: "pdb1" backupDestination: "NONE"
</pre></code>
