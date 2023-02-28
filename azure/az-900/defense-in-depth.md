---
description: >-
  https://www.youtube.com/watch?v=CHKS2FcEMek&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=37
---

# Defense in Depth

When thinking about security, imagine an onion with layers

* Data is the most important inner layer
* After that you might have application -> compute -> network -> perimeter -> identity/access -> physical security (responsibility of cloud provider)

CIA

* Confidentitaility - least privilidged access and only when you need it
* Integrity - nothing is tampering with data in rest or in transit
* Availability - being able to access your data even in the event of a DDoS attack or something similar
