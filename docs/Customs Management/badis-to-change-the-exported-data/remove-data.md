---
title: Remove data
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Sometimes you might want to remove certain data that has been determined by the standard logic.    

```Text Remove packages
DATA:         
          init_packages       TYPE /aeb/if_aes_pb_package_do=>tt_package_do,
           dlv_dos          TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
           dlv_do           TYPE REF TO /aeb/if_aes_pb_delivery_do.


*Loop over ICI-deliveries
dlv_dos = im_value->get_deliveries( ).
LOOP AT dlv_dos INTO dlv_do.

*Remove Packages
      dlv_do->set_packages( init_packages ).

ENDLOOP. 
```

<br>