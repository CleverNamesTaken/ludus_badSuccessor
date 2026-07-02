# Ansible Role: BadSuccessor ([Ludus](https://ludus.cloud))

An Ansible Role that installs creates BadSuccessor vulnerabilities in a Server 2025 Active Directory environment.

## Quickstart

```
ludus ansible roles add -d ../ludus_badSuccessor
ludus range config set -f minimal_range.yml
ludus range deploy
```

## Requirements

Must already have a Windows Server 2025 in the environment.

May need to swap out values for `ad_domain_functional_level` and `ad_forest_functional_level` in `/opt/ludus/ansible/server-config.yml` to Win2025 if you haven't built a Server 2025 DC before.


## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

      ludus_badSuccessor:
      #You can have multiple attach chains
        attackChains:
	#the writableUser is the user that can write on a target object
        - writableUser: domainuser
	#This is the object that we will impersonate
          targetObject: domainadmin
	#This is an OU that the writableUser has CreateChild rights on
          ou: badSuccessorOU

## Dependencies

None.

## Example Ludus Range Config

```yaml

ludus:
  - vm_name: "{{ range_id }}-BadSuccessor-DC"
    hostname: "{{ range_id }}-DC01-2025"
    template: win2025-server-x64-tpm-template
    vlan: 10
    ip_last_octet: 11
    ram_gb: 6
    cpus: 4
    windows:
      sysprep: true
    domain:
      fqdn: ludus.domain
      role: primary-dc
    roles:
      - CleverNamesTaken.ludus_BadSuccessor
    role_vars:
      ludus_badSuccessor:
        attackChains:
        - writableUser: domainuser
          targetObject: domainadmin
          ou: badSuccessorOU
```

## License

MIT

## Author Information

This role was created by [CleverNamesTaken](https://github.com/CleverNamesTaken), for [Ludus](https://ludus.cloud/).

# Credit

Much thanks to [Logan Goins](https://github.com/logangoins) for his guidance, encouragement, and excellent research.
