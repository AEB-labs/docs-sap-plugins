---
title: Change profile ID for VKORG
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Just as a short example, we replace the profile ID by one if a specific sales organization is used.
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  profile_id TYPE /aeb/if_80_pb_profile_id_do=>t_profile_id.\n\nIF im_vbrk-vkorg = 'VKORG'. \"or im_likp\n  profile_id = 'INV_FOR_VKORG'.\n  im_profile_id_do->set_profile_id( profile_id ).\nENDIF.",
      "language": "text",
      "name": "Change for VKORG"
    }
  ]
}
[/block]