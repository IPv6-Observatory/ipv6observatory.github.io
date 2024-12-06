---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---


The IPv6 Observatory is a community-oriented research effort that provides data
about active IPv6 networks. 

## Methodology

The IPv6 Observatory learns about active IPv6 networks through *passive* means
&mdash; our vantage points do not initiate any active scans.

To date, we support learning active networks through running Network Time
Protocol (NTP) servers, though we are actively exploring other methods for
address discovery. Our methodology and some results are described in the 2023
SIGCOMM paper [*IPv6 Hitlists at Scale: Be Careful What You Wish
For*](https://dl.acm.org/doi/pdf/10.1145/3603269.3604829).

## Privacy

As opposed to their IPv4 siblings, IPv6 addresses are (potentially)
significantly more privacy-sensitive.

Despite being discouraged from use in Globally Unique Addresses (GUAs) for over
20 years, many IPv6 addresses embed the MAC address of one of their network
interfaces &mdash; so-called Extended Unique Identifier - 64 (EUI-64) addresses.
Because MAC addresses are (typically) globally-unique, these types of addresses
permit tracking as a device changes networks over time. To discourage the use of
EUI-64 addresses, we provide counts of the number of unique MAC addresses we
observe from various manufacturers in our data.

Further, when devices generate *random* Interface Identifiers (IIDs) for their
IPv6 addresses &mdash; so-called SLAAC with Privacy Extensions, the
best-practice for IPv6 clients &mdash; the generated addresses are
highly-entropic, and are unlikely to ever be used by another IPv6 client.

For these reasons, we do not publish the full IPv6 addresses of the clients that
interact with IPv6 Observatory infrastructure.

## Data

### Active IPv6 Prefixes

This data set contains lists of active IPv6 networks we observe passively. We
strive to publish this data on a weekly basis.



### EUI-64 OUIs

This data set contains the count, OUI, and OUI vendor (as resolved by the [IEEE
OUI registration list](https://standards-oui.ieee.org/oui.txt)) if applicable.
Note that some OUIs do not resolve to a vendor; we suspect this may be due to
devices that use MAC address randomization that are using their
randomly-generated MAC address to generate an EUI-64 IPv6 address.

## Attribution

To cite this project, please use the original SIGCOMM 2023 paper:

```bibtex
@inproceedings{rye2023ipv6,
  title="{IPv6 Hitlists at Scale: Be Careful What You Wish For}",
  author={Rye, Erik and Levin, Dave},
  booktitle={Proceedings of the ACM SIGCOMM 2023 Conference},
  year={2023}
}
```

## Contribute

Do you have sources of active IPv6 networks you would like to share with the
community? We'd love to partner with you! 
