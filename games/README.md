# Floorbit's own rules list

One file per application: `<id>.yaml`, named after its id. The publish workflow checks every file
the way a Floorbit checks a rules file, signs the list with the index key and publishes it as
`applications.json` next to `index.json`. Floorbit's
[docs/applications.md](https://github.com/bitsandbricks-ch/floorbit/blob/master/docs/applications.md)
describes the fields.

```yaml
id: example-arena            # lower case, digits and dashes; a built-in entry's id replaces it
name: Example Arena
kind: game                   # game | launcher | sitegroup
category: gaming             # gaming | streaming | social | downloads | updates | web | other
part_of: steam               # games only: the launcher it runs from
sites: [example.com]         # domains; Floorbit matches by the registered domain
ports: [udp/7777]            # protocol/port or protocol/from-to
cidr: [198.51.100.0/28]
source: https://example.com/where-these-facts-come-from   # required: a link or a note
```

Add only what you have checked against real traffic or the publisher's own documentation.
