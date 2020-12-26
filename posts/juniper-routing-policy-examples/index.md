---
title: Juniper Routing Policy Examples
description: Configuration examples for routing policies in Juniper devices
Author: Marios Zindilis
first-published: 2014-01-20
---

Match Neighbor IP
-----------------

    policy-options {
        policy-statement bgp-incoming {
            term from-customer-ACME {
                from neighbor 1.2.3.4;
                then reject;
            }
        }
    }

Multiple Match Conditions
-------------------------

This policy will accept routes from a specific neighbor only if they 
are tagged with a specific community:

    policy-options {
        policy-statement bgp-incoming {
            term from-customer-ACME-accept {
                from {
                    neighbor 1.2.3.4;
                    community A1com;
                }
                then {
                    accept;
                }
            term from-customer-ACME-reject {
                from neighbor 1.2.3.4;
                then reject;
                }
            }
        }
    }
