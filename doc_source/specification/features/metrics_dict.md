## Metrics Dictionary

The "metrics" dictionary contains the values and metadata relevant for the 
metrics associated with the parent trace. One challenge with ground motion
metric data is that different metrics exhibit very different structure. For
example, some metrics are simply scalars (e.g., PGA) while others are arrays
(e.g., response spectra, which are a function of both oscillator period and
the assumed percent of critical damping).

While it is possible to list each metric as a separate element in a list, there
are two main drawbacks to doing so:
  1. It is inefficient from a file size perspective, and 
  2. It is difficult to parse because the properties will be different for each
     metric.

Thus, we have designed a general metrics data structure with the goals of
being easy to extend to a wide range of metric data structures, consistency in
how the metrics are stored to make parsing the data more straight-forward, and 
minimizing the file size. 

The dictionary has the following required keys:

**properties**
:  A dictionary with the following keys <i>(dictionary; required)</i>:

   **description**
   :  Metric description <i>(string; required)</i>.

   **name**
   :  Metric name <i>(string; required)</i>.

   **units**
   :  Metric units <i>(string; required)</i>.

   **provenance_ids**
   :  A list of strings giving provenance IDs relevant to this trace 
      <i>(list; optional)</i>.

**dimensions**
:  A dictionary with the following keys <i>(dictionary; required)</i>:

   **number**
   :  The number of dimentions for this metric (<code>n</code>) 
      <i>(integer; required)</i>.

   **names**
   :  A list of strings describing the metric dimension names; length must 
      be equal to the number of metric dimentions <code>n</code>
      <i>(list; required)</i>.

   **units**
   :  A list of strings describing the units of the metric dimensions; 
      length must be equal to the number of metric dimentions 
      <code>n</code> <i>(list; required)</i>.

   **values**
   :  An array giving the values of the dimensions; the first dimension of
      the array must equal the number of metric dimentions <code>n</code>;
      the lengths of each constituent array are are 
      <code>[p, q, ...]</code> <i>(array; required)</i>.

**values**
:  An array giving the metric values; the dimensions of the array must be 
   equal to the length of each of the dimention value arrays 
   <code>p x q x ...</code> <i>(array; required)</i>.

An example metrics dictionary:

```{code-block} json
---
force: true
---
  ...
  "traces": [
      {
          ...
          "metrics": [
              {
                  "properties": {
                      "description": "Spectral acceleration",
                      "name": "SA",
                      "units": "g",
                      "provenance_ids": [
                          "seis_prov:sp000_sa_0000000",
                          "seis_prov:sp000_pp_0000000",
                          "seis_prov:sp000_og_0000000"
                      ]
                  },
                  "dimensions": {
                      "number": 2,
                      "names": [
                          "critical damping",
                          "period"
                      ],
                      "units": ["%", "s"],
                      "values": [
                          [5.0, 10.0, 20.0],
                          [0.5, 1.0]
                      ]
                  },
                  "values": [
                      [2.3, 2.0],
                      [1.6, 1.4],
                      [2.0, 1.8]
                  ]
              }
          ]
      }
  ]

```


