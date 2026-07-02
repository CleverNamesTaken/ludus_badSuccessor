# Ansible Role: BadSuccessor ([Ludus](https://ludus.cloud))

An Ansible Role that creates BadSuccessor vulnerabilities in a Server 2025 Active Directory environment.

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

Available variables are listed below:

| VariableName | Type | Example | Description |
|--------------|------|---------|-------------|
| ludus_badSuccessor | Group | N/A | ludus_badSuccessor variables go under this group -- right now only attackChains |
| attackChains | Group | N/A | In theory you can build out a number of attack chains -- put them each in here |
| writableUser | String | domainuser | This is the user that has at least msDS-SupersededManagedAccountLink and msDS-SupersededServiceAccountState write privileges on a target object |
| targetObject | String | domainadmin | This is the target that we will be able to impersonate with BadSuccessor.  Could be a machine account, service account, or a user account |
| ou | String | badSuccessorOU | This is an OU that the writableUser has CreateChild rights on |

## Dependencies

This role assumes that the "writable users" and target objects already exist in the environment.  This role will create an OU if it doesn't already exist and ensure that the writable users have CreateChild access on the OU.

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
      - CleverNamesTaken.ludus_badSuccessor
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
