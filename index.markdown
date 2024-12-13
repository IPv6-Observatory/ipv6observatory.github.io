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

[2024-12-01-48s.txt.bz2](https://data.ipv6observatory.org/data/prefixes/2024-12-01-48s.txt.bz2)

For older data, see the [archive](https://data.ipv6observatory.org/data/prefixes/)

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

[2024-12-01-ouis.csv.bz2](https://ipv6observatory.org/data/ouis/2024-12-01-ouis.csv.bz2)

For older data, see the [archive](https://data.ipv6observatory.org/data/ouis/)

## Vantage Points

To ensure a broad geographic distribution, we operate 30 vantage points in 22
countries as of December 2024.

### Vantage Point Locations 

| Country          | Vantage Points |
|------------------|----------------|
|  Australia :australia: |  1  | 
|  Brazil :brazil: | 1              |
|  Cyprus :cyprus: | 1              |
|  Estonia :estonia: | 1              |
|  France :france: | 1              |
|  Germany :germany: | 1              |
|  Hong Kong :hong_kong: | 2              |
|  Hungary :hungary: | 2              |
|  India :india: | 1              |
|  Israel :israel: | 2              |
|  Kazakhstan :kazakhstan: | 2              |
|  Poland :poland: | 1              |
|  Serbia :serbia: | 2              |
|  Singapore :singapore: | 1              |
|  South Africa :south_africa: | 1              |
|  South Korea :kr: | 1              |
|  Spain :es: | 1              |
|  Türkiye :tr: | 1              |
|  Ukraine :ukraine: | 2              |
|  United Arab Emirates :united_arab_emirates: | 2              |
|  United Kingdom :uk: | 1              |
|  United States :us: | 2              |

### Vantage Point Map

<!-- Code from d3-graph-gallery.com -->
<meta charset="utf-8">

<!-- Load d3.js -->
<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v2.min.js"></script>

<!-- Create an element where the map will take place -->
<svg id="world-vp-map" width="800" height="300"></svg>

<script>

// The svg
var svg = d3.select("svg"),
  width = +svg.attr("width"),
  height = +svg.attr("height");

// Map and projection
var path = d3.geoPath();
var projection = d3.geoMercator()
  .scale(70)
  .center([0,20])
  .translate([width / 2, height / 2]);

// Data and color scale
var data = d3.map();
var colorScale = d3.scaleThreshold()
  .domain([1, 2, 3])
  .range(d3.schemeGreens[3]);

// Load external data and boot
d3.queue()
  .defer(d3.json, "https://ipv6observatory.org/vps/world.geojson")
  .defer(d3.csv, "https://ipv6observatory.org/vps/world_vps.csv", function(d) { data.set(d.code, +d.pop); })
  .await(ready);

function ready(error, topo) {

  // Create a tooltip element
  var tooltip = d3.select("body").append("div")
    .attr("class", "tooltip")
    .style("position", "absolute")
    .style("visibility", "hidden")
    .style("background", "white")
    .style("padding", "5px")
    .style("border", "1px solid #ccc")
    .style("border-radius", "4px")
    .style("box-shadow", "0 0 10px rgba(0, 0, 0, 0.1)");

  let mouseOver = function(d) {
 
     tooltip.style("visibility", "visible")
      .html(d.properties.name + "<br>VPs: " + (data.get(d.id) || 0).toLocaleString());

    d3.selectAll(".Country")
      .transition()
      .duration(200)
      .style("opacity", .5)
  }

  let mouseLeave = function(d) {
    tooltip.style("visibility", "hidden");

    d3.selectAll(".Country")
      .transition()
      .duration(200)
      .style("opacity", .8)
  }

  // Draw the map
  svg.append("g")
    .selectAll("path")
    .data(topo.features)
    .enter()
    .append("path")
      // draw each country
      .attr("d", d3.geoPath()
        .projection(projection)
      )
      // set the color of each country
      .attr("fill", function (d) {
        d.total = data.get(d.id) || 0;
        return colorScale(d.total);
      })
      .style("stroke", "transparent")
      .attr("class", function(d){ return "Country" } )
      .style("opacity", .8)
      .on("mouseover", mouseOver )
      .on("mouseleave", mouseLeave )
    }

</script>

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

[Erik Rye](https://erikrye.com) (University of Maryland): rye at umd.edu<br>
[Dave Levin](https://www.cs.umd.edu/~dml) (University of Maryland): dml at cs.umd.edu
