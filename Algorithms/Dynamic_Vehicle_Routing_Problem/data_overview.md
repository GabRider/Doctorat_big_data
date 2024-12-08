# Data Overview
The structure of the file for a vehicle routing problem (VRP), contains information for modeling and solving the problem.
## Headers
|Name|Description| format|
|----|-----------|-------|
|VRPTEST 1.0| Indicates the file format version. ||
|COMMENT| Provides additional information about the file, such as the source or known objectives. |str|
|NAME| Identifier for the problem instance. |str|
|NUM_DEPOTS| Number of depot |int|
|NUM_CAPACITIES| Number of constraints on capacity (e.g., weight or volume). |int|
|NUM_VISITS| Number of customer locations requiring service. |int|
|NUM_LOCATIONS| Total locations, including depots and visits. |int|
|NUM_VEHICLES| Number of available vehicles. |int|
|CAPACITIES| Maximum capacity of each vehicle. |int|

## Sections 
Contains detailed information related to the problem.

### Depots 

Contain the **ID(integer)** of the location that is considered as a depot.
``` 
DEPOTS
0 
34
```
### Customers request
Contain the **customer location(ID)** and the **weight** of his package (negative values). 

Example:
- Location _1_ has a weight package  of _3_ Kg.
- Location _3_ has a weight package of _7_ Kg.
```
DEMAND_SECTION
1 -3
...
3 -7
```
### Location
Provides the **coordinates (x, y)** for each location, including depots and customer sites.

``` 
LOCATION_COORD_SECTION
  0 5332 1525
  ...
  100 5323 4325
```
### Depot location
Maps **depot IDs** to their corresponding **location IDs**.

Example:
- Depot _0_ is located at location _0_.

``` 
DEPOT_LOCATION_SECTION
  0 0
```
### Visit location
Maps **visit IDs** to their corresponding **location IDs**.

Example:
- Visit _1_ corresponds to location _1_.
- Visit _2_ corresponds to location _2_.

``` 
VISIT_LOCATION_SECTION
  1 1
  ...
  1000 1000
```
### Visit duration
Specifies the **service time** (in arbitrary units) required at **each location**.

Example:
- Visit _1_ requires _95_ units of time.
```
DURATION_SECTION
1 95
2 95
3 95
...
```
### Depot availability
Defines time windows for depot operations.

Example:
- Depot _0_ is available from _0_ to _4816_ time units.
```
DEPOT_TIME_WINDOW_SECTION
0 0 4816
...
```
### Visit availability
Lists the **earliest time** for **each location** can be serviced.

Example:
- Location _1_ is available at _9_ time units
- Location _2_ is available at _16_ time unites
```
TIME_AVAIL_SECTION
1 9
2 16
3 26
...
```
### End of file
 Marks the end of the file.

 ``` 
 EOF
 ```
