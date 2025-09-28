---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

{:refdef: style="text-align: center;"}
![ipv6observatory](ipv6-observatory.png){: width="350" .center}
{: refdef}


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
20 years, many IPv6 addresses contain the MAC address of one of the device's
network interfaces in the lower 64 bits &mdash; so-called
Extended Unique Identifier - 64 (EUI-64) IPv6 addresses.  Because MAC addresses
are (typically) globally-unique, these types of addresses
permit tracking as a device changes networks over time. To discourage the use of
EUI-64 addresses, we provide counts of the number of unique MAC addresses we
observe from various manufacturers in our data.

Further, when devices generate *random* Interface Identifiers (IIDs) for their
IPv6 addresses &mdash; so-called SLAAC with Privacy Extensions, the
best-practice for IPv6 clients &mdash; the generated addresses are
highly-entropic, and are unlikely to ever be used by another IPv6 client.

For these reasons, we do not publish the full IPv6 addresses of the clients that
interact with IPv6 Observatory infrastructure. Instead, we aggregate observed
addresses to the encompassing /48 network.

## Data

### Active IPv6 Prefixes

This data set contains lists of active /48 IPv6 networks we observe passively.
Each line contains a zero-expanded /48 network (e.g. 2001:0db8:000) with the
remaining 80 bits and prefix omitted.  We strive to publish this data on a
weekly basis; the naming convention for our files is `YYYY-MM-DD-48s.txt`, where
that date is the beginning of the 7-day period at 00:00:00 UTC.

[2025-09-07-48s.txt.bz2](https://data.ipv6observatory.org/current/2025-09-07-48s.txt.bz2)

For older data, first [register](#archive-data), then see the
[archive](https://data.ipv6observatory.org/data/prefixes/).

### EUI-64 OUIs

This data set contains the count, OUI, and OUI vendor (as resolved by the [IEEE
OUI registration list](https://standards-oui.ieee.org/oui.txt)) if applicable.
Note that some OUIs do not resolve to a vendor; we suspect that in some cases
this may be due to devices that use MAC address randomization that are using
their randomly-generated MAC address to generate an EUI-64 IPv6 address. In
others, particularly unassigned OUIs with *many* unique embedded MAC address
observations, we surmise that a vendor is simply using an allocation without it
being registered to them.  We strive to publish this data on a weekly basis; the
naming convention for our files is `YYYY-MM-DD-ouis.csv`, where that date is the
beginning of the 7-day period at 00:00:00 UTC. 

[2025-09-07-ouis.csv.bz2](https://data.ipv6observatory.org/current/2025-09-07-ouis.csv.bz2)

For older data, first [register](#archive-data), then see the
[archive](https://data.ipv6observatory.org/data/ouis/).

## NTP Vantage Points

To ensure broad geographic distribution, we operate 42 vantage points in 30
countries as of September 2025.

### NTP Vantage Point Locations 

| Country          | Vantage Points |
| :---:            | :-------------:|
|  Australia :australia: |  1  | 
|  Brazil :brazil: | 1              |
|  Bulgaria :bulgaria: | 2              |
|  Canada :canada: | 2              |
|  Chile :chile: | 1              |
|  Cyprus :cyprus: | 1              |
|  Estonia :estonia: | 1              |
|  France :fr: | 1              |
|  Germany :de: | 2              |
|  Hong Kong :hong_kong: | 2              |
|  Hungary :hungary: | 2              |
|  India :india: | 2              |
|  Israel :israel: | 2              |
|  Japan :jp: | 1              |
|  Kazakhstan :kazakhstan: | 2              |
|  Mexico :mexico: | 1              |
|  Norway :norway: | 1              |
|  Poland :poland: | 1              |
|  Russia :ru: | 1              |
|  Serbia :serbia: | 2              |
|  Singapore :singapore: | 1              |
|  South Africa :south_africa: | 1              |
|  South Korea :kr: | 1              |
|  Spain :es: | 1              |
|  Sweden :sweden: | 1              |
|  Türkiye :tr: | 1              |
|  Ukraine :ukraine: | 2              |
|  United Arab Emirates :united_arab_emirates: | 2              |
|  United Kingdom :uk: | 1              |
|  United States :us: | 2              |

## Archive Data

To obtain our historical data, we ask that you send us a brief
[email](mailto:rye@umd.edu) to register. We use this information to track usage
statistics.

We currently maintain the following datasets:

| Dataset | Periodicity | Start | 
| :-----: | :---------: | :---: |
| NTP /48s  | Weekly    | July 2024 |
| NTP EUI-64 OUI Counts | Weekly    | July 2024 |

## Attribution

To cite this project, please use reference for the SIGCOMM 2023 paper:

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

## FAQ

1. "How is this different from the IPv6 Hitlist?"

    There are several data sets that are complementary to ours.  [The IPv6
    Hitlist](https://ipv6hitlist.github.io/) reports IPv6 addresses that are
    responsive to one of several protocol scans (e.g., ICMPv6, DNS, and common web
    ports), while our data reports networks that we observe to be allocated to hosts
    through passive means. See our [SIGCOMM
    2023](https://dl.acm.org/doi/pdf/10.1145/3603269.3604829) paper for an analysis
    of how our data complements The IPv6 Hitlist, as well as active measurements
    by [CAIDA](https://www.caida.org/).

2. "Why are there bogon/Martian networks in your data?"

    Our data set may include bogon or Martian networks due to a lack of
    filtering by the network operators from which the traffic originates.

## Contact

[Erik Rye](https://erikrye.com) (Johns Hopkins University): rye at jhu.edu<br>
[Dave Levin](https://www.cs.umd.edu/~dml) (University of Maryland): dml at cs.umd.edu
