README
====

This project demonstrates how to use Ansible and the JCliff collection to deploy and activate
a new JDBC driver inside Wildfly.

Prerequesites
----

To run this demo, you'll need a RHEL 8.4 system, registered.

* Install Ansible 2.9 installed, either using EPEL or preferably using subscription-manager:
    subscription-manager --enable
* Install Ansible dependencies using Galaxy:
    $ ansible-galaxy collection install -r collections/requirements.yml

You can perform all those operation by simply running the setup.sh script on the target host.

Execute the playbook
----

Simply run the playbook, from the target host, and provide it the required RHN credentials:

    $ ansible-playbook playbook.yml

Running the playbook will take several minutes depending on system capabilities and the newtork bandwidth.

Once the playbook has ran successfully, the target system will be running wildfly as a systemd service:

    # systemctl status wildfly

You can confirm the server is working by executing the following curl statement from the target host:

    # curl -v http://localhost:8080/

To verify that the JDBC driver for Postgresql has been deployed and enabled:

    $ /opt/wildfly/wildfly-19.0.0.Final/bin/jboss-cli.sh --connect --command='/subsystem=datasources/jdbc-driver=postgresql:read-resource'
    {
      "outcome" => "success",
      "result" => {
        "deployment-name" => undefined,
        "driver-class-name" => undefined,
        "driver-datasource-class-name" => undefined,
        "driver-major-version" => undefined,
        "driver-minor-version" => undefined,
        "driver-module-name" => "org.postgresql",
        "driver-name" => "postgresql",
        "driver-xa-datasource-class-name" => "org.postgresql.xa.PGXADataSource",
        "jdbc-compliant" => undefined,
        "module-slot" => undefined,
        "profile" => undefined,
        "xa-datasource-class" => undefined
      },
    }

