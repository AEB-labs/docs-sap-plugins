---
title: Basic concept of a compliance check
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: >-
    Now you can dive in the concrete checks or like to get results of the
    checks?

    Or just use some seperate functions.
  pages:
    - type: basic
      slug: adapt-screening-checks
      title: Adapt Screening checks
    - type: basic
      slug: check-or-not-1
      title: Adapt Export Controls checks
---
Compliance checks are triggered in different situations:
- when saving a business object (we call this "online check")
- in context of the Compliance Monitor 
- in context of the batch checks
- in context of asynchronous checks

During the compliance check, different sub-steps are executed:     
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4e7d4ff-Logic_CMP_SAP.png",
        "Logic_CMP_SAP.png",
        841,
        556,
        "#f08da6"
      ]
    }
  ]
}
[/block]
If a business object has more than one organizational unit, the all required checks are done for each organizational unit. The overall result decides, whether  the blocks are set or not.

If  a business object is not checked by Screening and not checked Export Controls, it's like there is no check in general. Keep that in mind  if you adapt the logic for checking a business object.