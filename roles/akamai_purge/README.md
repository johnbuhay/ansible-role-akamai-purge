Role Name
=========

This role hits the akamai REST api for certain purge related tasks.

Requirements
------------

Most tasks require authentication, you will be prompted using prompt.yml for username//password, expecting first.lastname which will resolve to first.lastname@example.com.
This user must have access to akamai control center.
Unfortunately you will always be prompted even if you are running akamai_query, which does not require authentication

Role Variables
--------------
  - arl
  - cpcode - is the default method of purging

Extra Variables
---------------

  - notify_team - use this to add flowdock notifications to another team specified in group_vars/global/team_profiles
  - akamai_action=invalidate - use this to overide the default value, which is 'remove' with 'invalidate'
  - queue_name - use this to override the default value, which is 'default', only other option is 'emergency'

Dependencies
------------

httplib2 must be installed: sudo pip install httplib2, a task has been added to do this to your local machine

Documentation
----------------
  - https://api.ccu.akamai.com/ccu/v2/docs/index.html
  - https://developer.akamai.com/api/purge/ccu/overview.html
  - https://developer.akamai.com/api
  - http://docs.ansible.com/flowdock_module.html

Example Playbook
----------------

This role is independently used.

Usage: ansible-playbook akamai.yml -i inventory/local --tags akamai_status
successful response looks like this:

```json
    {
        "httpStatus" : 200,
        "queueLength" : 17,
        "detail" : "The queue may take a minute to reflect new or removed requests.",
        "supportId" : "17QY1321286863376510-220300384
    }
```

Usage: ansible-playbook akamai.yml -i inventory/local --tags akamai_purge -vv


A purge request is submitted by sending a POST request to the /ccu/v2/queues/default or /ccu/v2/queues/emergency resource. 
The Content-Type must be application/json.

To construct the request object:

Optionally specify a JSON pair for type, which is either arl or cpcode. type defaults to “arl” if omitted.
With “objects” as the key, provide a JSON array of valid ARLs, URLs, or both, when “arl” is the type.
For “cpcode” type, supply an array of CP codes.

successful response to a purge request:

```json
    {
     "estimatedSeconds": 420,
     "progressUri": "/ccu/v2/purges/57799d8b-10e4-11e4-9088-62ece60caaf0",
     "purgeId": "57799d8b-10e4-11e4-9088-62ece60caaf0",
     "supportId": "17PY1405953363409286-284546144",
     "httpStatus": 201,
     "detail": "Request accepted.",
     "pingAfterSeconds": 420
    }
```

example error response is:

```json
    {
     "supportId": "17PY1234567890123456-123456789",
     "title": "queue is full",
     "httpStatus": 507,
     "detail": "User's queue is full",
     "describedBy": "https://api.ccu.akamai.com/ccu/v2/errors/queue-is-full"
    }
```

simple purge request of type arl looks like this:

```json
    {
        "objects" : [
            "http://www.example.com/graphics/picture.gif",
            "http://www.example.com/documents/brochure.pdf"
        ]
    }
```

purge request of type cpcode looks like this:

```json
    {
        "objects" : [6848, 44],
        "action": "remove",
        "type": "cpcode"
    }
```

response

```json
     {
     "estimatedSeconds": 420,
     "progressUri": "/ccu/v2/purges/57799d8b-10e4-11e4-9088-62ece60caaf0",
     "purgeId": "57799d8b-10e4-11e4-9088-62ece60caaf0",
     "supportId": "17PY1405953363409286-284546144",
     "httpStatus": 201,
     "detail": "Request accepted.",
     "pingAfterSeconds": 420
    }
```

purge request status
successful response looks like this:

```json
    {
        "originalEstimatedSeconds": 480,
        "progressUri": "/ccu/v2/purges/142eac1d-99ab-11e3-945a-7784545a7784",
        "originalQueueLength": 6,
        "purgeId": "142eac1d-99ab-11e3-945a-7784545a7784",
        "supportId": "17SY1392844709041263-238396512",
        "httpStatus": 200,
        "completionTime": null,
        "submittedBy": "test1",
        "purgeStatus": "In-Progress",
        "submissionTime": "2014-02-19T21:16:20Z",
        "pingAfterSeconds": 60
    }
```

License
-------

BSD

Author Information
------------------

John Buhay
jnbuhaynyc+github@gmail.com
