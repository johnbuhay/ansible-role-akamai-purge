#  Akamai purge

This role uses akamai credentials to submit either ARL or CPCODE purges through the Akamai API.

### [This Ansible Role Documentation](roles/akamai_purge/README.md)
### Further documentation resources
  - https://api.ccu.akamai.com/ccu/v2/docs/index.html
  - https://developer.akamai.com/api/purge/ccu/overview.html
  - https://developer.akamai.com/api
  - http://docs.ansible.com/flowdock_module.html

### To check the akamai queue status
  ```bash
  ansible-playbook akamai.yml -i inventory/local --tags akamai_status
  ```

### To purge ARL's (asset resource location?)
  ```bash
  ansible-playbook akamai.yml -i inventory/local --tags akamai_purge_arl -vv --extra-vars arl=http://www.example.com/graphics/picture.gif,http://www.example.com/documents/brochure.pdf
  ```

### To purge CPCODE's
  ```bash
  ansible-playbook akamai.yml -i inventory/local --tags akamai_purge_cpcode --extra-vars cpcode=number,number1,number2
  ```

### To notify flowdock channels via team_profiles definition add the notify_team variable
  ```bash
  --extra-vars 'cpcode=number,anotherNumber notify_team=release_team'
  ```

### To check if an ARL is cached
  ```bash
  ansible-playbook akamai.yml -i inventory/local --tags akamai_query -vv --extra-vars arl=https://www.example.com/favicon.ico
  ```

  Example provided by akamai tech
  ```bash
  curl -i -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key" https://www.example.com/favicon.ico
  ```
