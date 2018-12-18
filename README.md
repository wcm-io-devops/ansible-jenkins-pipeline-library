# wcm_io_devops.jenkins_pipeline_library

This role is a utility roles to setup jenkins instances for the usage of
the wcm-io-devops
[jenkins-pipeline-library](https://github.com/wcm-io-devops/jenkins-pipeline-library).

This role installs all necessary plugins in specific versions which
ensures that the shared pipeline library is always running with the
latest compatible plugins.

:exclamation: Starting with version 2.138.1-1 the role will also
configure the script security to allow some signatures!

Optionally the role also directly setups a Jenkins instance using [geerlingguy.jenkins](https://github.com/geerlingguy/ansible-role-jenkins).

## Versioning

The Version number will follow the following versioning schema:

`[JenkinsVersion]-[ReleaseCount]`

So for example:
* `2.107.2-1` - first release for Jenkins 2.107.2
* `2.107.2-2` - second release for Jenkins 2.107.2
* `2.107.2-N` - nth release for Jenkins 2.107.2
* `2.121.3-1` - first release for Jenkins 2.121.3

## Requirements

This role requires Ansible 2.4 or higher and a running Jenkins on the
target instance.

## Role Variables

Available variables are listed below, along with their default values:

    jenkins_pipeline_library_jenkins_version: # see defaults.yaml for up-to-date value

The version of the jenkins when it is installed with the jenkins role dependency.

    jenkins_pipeline_library_jenkins_install: false

Controls if the jenkins will be installed by the jenkins role dependency.

    jenkins_pipeline_library_jenkins_process_user: jenkins

Linux jenkins user.

    jenkins_pipeline_library_jenkins_process_group: "{{ jenkins_pipeline_library_jenkins_process_user }}"

Linux group of jenkins user.

    jenkins_pipeline_library_jenkins_pkg_url: https://pkg.jenkins.io/debian-stable/binary

Package url for installing specific stable jenkins versions.

    jenkins_pipeline_library_admin_username: admin

Jenkins admin username.

    jenkins_pipeline_library_admin_password: admin

Jenkins admin password.

    jenkins_pipeline_library_jenkins_home: /var/lib/jenkins

Path to the jenkins directory.

    jenkins_pipeline_library_jenkins_hostname: localhost

Hostname of the jenkins instance.

    jenkins_pipeline_library_jenkins_port: 8080

HTTP port of the jenkins instance.

    jenkins_pipeline_library_jenkins_url_prefix: ""

Url prefix of the jenkins instance, e.g. when running in tomcat.

    jenkins_pipeline_library_jenkins_update_dir: "{{ jenkins_pipeline_library_jenkins_home }}/updates"

Path to the jenkins update directory.

    jenkins_pipeline_library_jenkins_base_url: "http://{{ jenkins_facts_jenkins_hostname }}:{{ jenkins_facts_jenkins_port }}{{ jenkins_facts_jenkins_url_prefix }}"

The base url of the jenkins instance.

    jenkins_pipeline_library_updates_expiration: 86400

Maximum seconds since the last jenkins plugin update check.

    jenkins_pipeline_library_updates_timeout: 60

Timeout for jenkins update operation.

    jenkins_pipeline_library_debug: false

When set to enable the role will log some debug information.

    jenkins_pipeline_library_plugins_present: [...]

Plugins and their versions that must be present for
jenkins-pipeline-library.

    jenkins_pipeline_library_approved_signatures_present:

Signatures that need to be approved for jenkins-pipeline-library,
otherwise the signatures will be blocked by script-security.

:bulb: Since this list is long please refer to
[defaults](defaults/main.yaml)

    jenkins_pipeline_library_plugins_absent: []

Plugins that must be absent for jenkins-pipeline-library.

## Dependencies

This role depends on the
[wcm_io_devops.jenkins_plugins](https://github.com/wcm-io-devops/ansible-jenkins-plugins)
role to install/uninstall the plugins needed by the
[jenkins-pipeline-library](https://github.com/wcm-io-devops/jenkins-pipeline-library)

In order to allow to run the
[jenkins-pipeline-library](https://github.com/wcm-io-devops/jenkins-pipeline-library)
with the Pipeline Supporting APIs 1.18+ (workflow-support) and with
Script Security ( script-security) 1.44+ this role also depends on
[wcm_io_devops.jenkins_script_plugin](https://github.com/wcm-io-devops/ansible-jenkins-script-security)
for whitelisting some signatures in the Groovy Sandbox.

For installing a Jenkins on the target instance the role is using
[geerlingguy.jenkins](https://github.com/geerlingguy/ansible-role-jenkins).

As transitive dependency this role uses the
[wcm_io_devops.jenkins_facts](https://github.com/wcm-io-devops/ansible-jenkins-facts)
role to retrieve the list of installed plugins from the Jenkins
instance.

## Example Playbook

Prepares the Jenkins instance for the use of the
https://github.com/wcm-io-devops/jenkins-pipeline-library.

	- hosts: jenkins
	  roles:
	    - role: wcm_io_devops.jenkins_pipeline_library

## License

Apache 2.0
