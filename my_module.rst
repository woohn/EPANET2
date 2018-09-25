[BACKDROP]
==========

   **Purpose:**

   Identifies a backdrop image and dimensions for the network map.

   **Formats:**

   **DIMENSIONS** *LLx LLy URx URy*

   **UNITS FEET/METERS/DEGREES/NONE**

   **FILE** *filename*

   **OFFSET** *X Y*

   **Definitions:**

   **DIMENSIONS** provides the X and Y coordinates of the lower-left and
   upper-right corners of the map’s bounding rectangle. Defaults are the
   extents of the nodal coordinates supplied in the [COORDINATES]
   section.

   **UNITS** specifies the units that the map’s dimensions are given in.
   Default is NONE.

   **FILE** is the name of the file that contains the backdrop image.

   **OFFSET** lists the X and Y distance that the upper-left corner of
   the backdrop image is offset from the upper-left corner of the map’s
   bounding rectangle. Default is zero offset.

   **Remarks:**

a. The [BACKDROP] section is optional and is not used at all when EPANET
   is run as a console application.

b. Only Windows Enhanced Metafiles and bitmap files can be used as
   backdrops.

[CONTROLS]
==========

   **Purpose:**

   Defines simple controls that modify links based on a single
   condition.

   **Format:**

   One line for each control which can be of the form:

   **LINK** linkID status **IF NODE** nodeID **ABOVE/BELOW** value

   **LINK** linkID status **AT TIME** time

   **LINK** linkID status **AT CLOCKTIME** clocktime **AM/PM**

   where:

+-----------------------+-----------------------+-----------------------+
|    linkID             |    =                  |    a link ID label    |
+=======================+=======================+=======================+
|    status             |    =                  |    OPEN or CLOSED, a  |
|                       |                       |    pump speed         |
|                       |                       |    setting, or a      |
|                       |                       |    control            |
+-----------------------+-----------------------+-----------------------+
|                       |                       |    valve setting      |
+-----------------------+-----------------------+-----------------------+
|    nodeID             |    =                  |    a node ID label    |
+-----------------------+-----------------------+-----------------------+
|    value              |    =                  |    a pressure for a   |
|                       |                       |    junction or a      |
|                       |                       |    water level for a  |
|                       |                       |    tank               |
+-----------------------+-----------------------+-----------------------+
|    time               |    =                  |    a time since the   |
|                       |                       |    start of the       |
|                       |                       |    simulation in      |
|                       |                       |    decimal hours or   |
|                       |                       |    in hours:minutes   |
|                       |                       |    format             |
+-----------------------+-----------------------+-----------------------+
|    clocktime          |    =                  |    a 24-hour clock    |
|                       |                       |    time               |
|                       |                       |    (hours:minutes)    |
+-----------------------+-----------------------+-----------------------+

..

   **Remarks:**

a. Simple controls are used to change link status or settings based on
   tank water level, junction pressure, time into the simulation or time
   of day.

b. See the notes for the [STATUS] section for conventions used in
   specifying link status and setting, particularly for control valves.

..

   **Examples:**

   [CONTROLS]

   ;Close Link 12 if the level in Tank 23 exceeds 20 ft. LINK 12 CLOSED
   IF NODE 23 ABOVE 20

   ;Open Link 12 if pressure at Node 130 is under 30 psi LINK 12 OPEN IF
   NODE 130 BELOW 30

   ;Pump PUMP02's speed is set to 1.5 at 16 hours into

   ;the simulation

   LINK PUMP02 1.5 AT TIME 16

   ;Link 12 is closed at 10 am and opened at 8 pm

   ;throughout the simulation

   LINK 12 CLOSED AT CLOCKTIME 10 AM LINK 12 OPEN AT CLOCKTIME 8 PM

[COORDINATES]
=============

   **Purpose:**

   Assigns map coordinates to network nodes.

   **Format:**

   One line for each node containing:

-  Node ID label

-  X-coordinate

-  Y-coordinate

..

   **Remarks:**

a. Include one line for each node displayed on the map.

b. The coordinates represent the distance from the node to an arbitrary
   origin at the lower left of the map. Any convenient units of measure
   for this distance can be used.

c. There is no requirement that all nodes be included in the map, and
   their locations need not be to actual scale.

d. A [COORDINATES] section is optional and is not used at all when
   EPANET is run as a console application.

..

   **Example:**

   [COORDINATES]

   ;Node X-Coord. Y-Coord

   ;-------------------------------

+------+-------+-----+
|    1 | 10023 | 128 |
+======+=======+=====+
|    2 | 10056 | 95  |
+------+-------+-----+

[CURVES]
========

   **Purpose:**

   Defines data curves and their X,Y points.

   **Format:**

   One line for each X,Y point on each curve containing:

-  Curve ID label

-  X value

-  Y value

..

   **Remarks:**

a. Curves can be used to represent the following relations:

   -  Head v. Flow for pumps

   -  Efficiency v. Flow for pumps

   -  Volume v. Depth for tanks

   -  Headloss v. Flow for General Purpose Valves

b. The points of a curve must be entered in order of increasing X-values
   (lower to higher).

c. If the input file will be used with the Windows version of EPANET,
   then adding a comment which contains the curve type and description,
   separated by a colon, directly above the first entry for a curve will
   ensure that these items appear correctly in EPANET’s Curve Editor.
   Curve types include PUMP, EFFICIENCY, VOLUME, and HEADLOSS. See the
   examples below.

..

   **Example:**

   [CURVES]

   ;ID Flow Head

   ;PUMP: Curve for Pump 1 C1 0 200

   C1 1000 100

   C1 3000 0

   ;ID Flow Effic.

   ;EFFICIENCY:

   E1 200 50

   E1 1000 85

   E1 2000 75

   E1 3000 65

[DEMANDS]
=========

   **Purpose:**

   Supplement to [JUNCTIONS] section for defining multiple water demands
   at junction nodes.

   **Format:**

   One line for each category of demand at a junction containing:

-  Junction ID label

-  Base demand (flow units)

-  Demand pattern ID (optional)

-  Name of demand category preceded by a semicolon (optional)

..

   **Remarks:**

a. Only use for junctions whose demands need to be changed or
   supplemented from entries in [JUNCTIONS] section.

b. Data in this section replaces any demand entered in [JUNCTIONS]
   section for the same junction.

c. Unlimited number of demand categories can be entered per junction.

a. If no demand pattern is supplied then the junction demand follows the
   Default Demand Pattern specified in the [OPTIONS] section or Pattern
   1 if no default pattern is specified. If the default pattern (or
   Pattern 1) does not exist, then the demand remains constant.

..

   **Example:**

   [DEMANDS]

   ;ID Demand Pattern Category

   ;---------------------------------

+---------+--------+-----+--------------+
|    J1   |    100 | 101 |    ;Domestic |
+=========+========+=====+==============+
|    J1   |    25  | 102 |    ;School   |
+---------+--------+-----+--------------+
|    J256 |    50  | 101 |    ;Domestic |
+---------+--------+-----+--------------+

[EMITTERS]
==========

   **Purpose:**

   Defines junctions modeled as emitters (sprinklers or orifices).

   **Format:**

   One line for each emitter containing:

-  Junction ID label

-  Flow coefficient, flow units at 1 psi (1 meter) pressure drop

..

   **Remarks:**

a. Emitters are used to model flow through sprinkler heads or pipe
   leaks.

b. Flow out of the emitter equals the product of the flow coefficient
   and the junction pressure raised to a power.

c. The power can be specified using the EMITTER EXPONENT option in the
   [OPTIONS] section. The default power is 0.5, which normally applies
   to sprinklers and nozzles.

d. Actual demand reported in the program's results includes both the
   normal demand at the junction plus flow through the emitter.

e. An [EMITTERS] section is optional.

[ENERGY]
========

   **Purpose:**

   Defines parameters used to compute pumping energy and cost.

   **Formats:**

   **GLOBAL PRICE/PATTERN/EFFIC** *value* **PUMP** *PumpID*
   **PRICE/PATTERN/EFFIC** *value* **DEMAND CHARGE** *value*

   **Remarks:**

c. Lines beginning with the keyword **GLOBAL** are used to set global
   default values of energy price, price pattern, and pumping efficiency
   for all pumps.

d. Lines beginning with the keyword **PUMP** are used to override global
   defaults for specific pumps.

e. Parameters are defined as follows:

   -  **PRICE** = average cost per kW-hour,

   -  **PATTERN** = ID label of time pattern describing how energy price
      varies with time,

   -  **EFFIC** = either a single percent efficiency for global setting
      or the ID label of an efficiency curve for a specific pump,

   -  **DEMAND CHARGE** = added cost per maximum kW usage during the
      simulation period.

f. The default global pump efficiency is 75% and the default global
   energy price is 0.

g. All entries in this section are optional. Items offset by slashes (/)
   indicate allowable choices.

..

   **Example:**

   [ENERGY]

   GLOBAL PRICE 0.05 ;Sets global energy price GLOBAL PATTERN PAT1 ;and
   time-of-day pattern PUMP 23 PRICE 0.10 ;Overrides price for Pump 23

   PUMP 23 EFFIC E23 ;Assigns effic. curve to Pump 23

[JUNCTIONS]
===========

   **Purpose:**

   Defines junction nodes contained in the network.

   **Format:**

   One line for each junction containing:

-  ID label

-  Elevation, ft (m)

-  Base demand flow (flow units) (optional)

-  Demand pattern ID (optional)

..

   **Remarks:**

b. A [JUNCTIONS] section with at least one junction is required.

c. If no demand pattern is supplied then the junction demand follows the
   Default Demand Pattern specified in the [OPTIONS] section or Pattern
   1 if no default pattern is specified. If the default pattern (or
   Pattern 1) does not exist, then the demand remains constant.

d. Demands can also be entered in the [DEMANDS] section and include
   multiple demand categories per junction.

..

   **Example:**

   [JUNCTIONS]

   ;ID Elev. Demand Pattern

   ;------------------------------

+-------+--------+-------+---------+---------------------------------+
|    J1 |    100 |    50 |    Pat1 |                                 |
+=======+========+=======+=========+=================================+
|    J2 |    120 |    10 |         |    ;Uses default demand pattern |
+-------+--------+-------+---------+---------------------------------+
|    J3 |    115 |       |         |    ;No demand at this junction  |
+-------+--------+-------+---------+---------------------------------+

[LABELS]
========

   **Purpose:**

   Assigns coordinates to map labels.

   **Format:**

   One line for each label containing:

-  X-coordinate

-  Y-coordinate

-  Text of label in double quotes

-  ID label of an anchor node (optional)

..

   **Remarks:**

a. Include one line for each label on the map.

b. The coordinates refer to the upper left corner of the label and are
   with respect to an arbitrary origin at the lower left of the map.

c. The optional anchor node anchors the label to the node when the map
   is re-scaled during zoom-in operations.

d. The [LABELS] section is optional and is not used at all when EPANET
   is run as a console application.

..

   **Example:**

   [LABELS]

   ;X-Coord. Y-Coord. Label Anchor

   ;-----------------------------------------------

+----------+----------+-----------------+--------+
|    1230  |    3459  |    “Pump 1”     |        |
+==========+==========+=================+========+
|    34.57 |    12.75 |    “North Tank” |    T22 |
+----------+----------+-----------------+--------+

[MIXING]
========

   **Purpose:**

   Identifies the model that governs mixing within storage tanks.

   **Format:**

   One line per tank containing:

-  Tank ID label

-  Mixing model (MIXED, 2COMP, FIFO, or LIFO)

-  Compartment volume (fraction)

..

   **Remarks:**

a. Mixing models include:

   -  Completely Mixed (MIXED)

   -  Two-Compartment Mixing (2COMP)

   -  Plug Flow (FIFO)

   -  Stacked Plug Flow (LIFO)

b. The compartment volume parameter only applies to the two-compartment
   model and represents the fraction of the total tank volume devoted to
   the inlet/outlet compartment.

c. The [MIXING] section is optional. Tanks not described in this section
   are assumed to be completely mixed.

..

   **Example:**

   [MIXING]

   ;Tank Model

   ;----------------------- T12 LIFO

   T23 2COMP 0.2

.. _options-1:

[OPTIONS]
=========

   **Purpose:**

   Defines various simulation options.

   **Formats:**

   **UNITS CFS/GPM/MGD/IMGD/AFD/ LPS/LPM/MLD/CMH/CMD**

   **HEADLOSS H-W/D-W/C-M**

   **HYDRAULICS USE/SAVE** filename

   **QUALITY NONE/CHEMICAL/AGE/TRACE id**

   **VISCOSITY** value

   **DIFFUSIVITY** value

   **SPECIFIC GRAVITY** value

   **TRIALS** value

   **ACCURACY** value

   **UNBALANCED STOP/CONTINUE/CONTINUE n**

   **PATTERN** id

   **DEMAND MULTIPLIER** value

   **EMITTER EXPONENT** value

   **TOLERANCE** value

   **MAP** filename

   **Definitions:**

   **UNITS** sets the units in which flow rates are expressed where:

   **CFS** = cubic feet per second **GPM** = gallons per minute **MGD**
   = million gallons per day **IMGD** = Imperial MGD

   **AFD** = acre-feet per day

   **LPS** = liters per second

   **LPM** = liters per minute

   **MLD** = million liters per day **CMH** = cubic meters per hour
   **CMD** = cubic meters per day

   For **CFS, GPM, MGD, IMGD**, and **AFD** other input quantities are
   expressed in US Customary Units. If flow units are in liters or cubic
   meters then Metric Units must be used for all other input quantities
   as

   well. (See Appendix A. Units of Measurement). The default flow units
   are **GPM**.

   **HEADLOSS** selects a formula to use for computing head loss for
   flow through a pipe. The choices are the Hazen-Williams (**H-W**),
   Darcy-Weisbach (**D-W**), or Chezy-Manning (**C-M**) formulas. The
   default is **H-W**.

   The **HYDRAULICS** option allows you to either **SAVE** the current
   hydraulics solution to a file or **USE** a previously saved
   hydraulics solution. This is useful when studying factors that only
   affect water quality behavior.

   **QUALITY** selects the type of water quality analysis to perform.
   The choices are **NONE, CHEMICAL, AGE**, and **TRACE**. In place of
   **CHEMICAL** the actual name of the chemical can be used followed by
   its concentration units (e.g., **CHLORINE mg/L**). If **TRACE** is
   selected it must be followed by the ID label of the node being
   traced. The default selection is **NONE** (no water quality
   analysis).

   **VISCOSITY** is the kinematic viscosity of the fluid being modeled
   relative to that of water at 20 deg. C (1.0 centistoke). The default
   value is 1.0.

   **DIFFUSIVITY** is the molecular diffusivity of the chemical being
   analyzed relative to that of chlorine in water. The default value is
   1.0. Diffusivity is only used when mass transfer limitations are
   considered in pipe wall reactions. A value of 0 will cause EPANET to
   ignore mass transfer limitations.

   **SPECIFIC GRAVITY** is the ratio of the density of the fluid being
   modeled to that of water at 4 deg. C (unitless).

   **TRIALS** are the maximum number of trials used to solve network
   hydraulics at each hydraulic time step of a simulation. The default
   is 40.

   **ACCURACY** prescribes the convergence criterion that determines
   when a hydraulic solution has been reached. The trials end when the
   sum of all flow changes from the previous solution divided by the
   total flow in all links is less than this number. The default is
   0.001.

   **UNBALANCED** determines what happens if a hydraulic solution cannot
   be reached within the prescribed number of **TRIALS** at some
   hydraulic time step into the simulation. **"STOP"** will halt the
   entire analysis at that point. **"CONTINUE"** will continue the
   analysis with a warning message issued. **"CONTINUE n"** will
   continue the search for a solution for another "n" trials with the
   status of all links held fixed at their current settings. The
   simulation will be continued at this point with a message issued
   about whether convergence was achieved or not. The default choice is
   **"STOP"**.

   **PATTERN** provides the ID label of a default demand pattern to be
   applied to all junctions where no demand pattern was specified. If no
   such pattern exists in the [PATTERNS] section then by default the
   pattern consists of a single multiplier equal to 1.0. If this option
   is not used, then the global default demand pattern has a label of
   "1".

   The **DEMAND MULTIPLIER** is used to adjust the values of baseline
   demands for all junctions and all demand categories. For example, a
   value of 2 doubles all baseline demands, while a value of 0.5 would
   halve them. The default value is 1.0.

   **EMITTER EXPONENT** specifies the power to which the pressure at a
   junction is raised when computing the flow issuing from an emitter.
   The default is 0.5.

   **MAP** is used to supply the name of a file containing coordinates
   of the network's nodes so that a map of the network can be drawn. It
   is not used for any hydraulic or water quality computations.

   **TOLERANCE** is the difference in water quality level below which
   one can say that one parcel of water is essentially the same as
   another. The default is 0.01 for all types of quality analyses
   (chemical, age (measured in hours), or source tracing (measured in
   percent)).

   **Remarks:**

a. All options assume their default values if not explicitly specified
   in this section.

b. Items offset by slashes (/) indicate allowable choices.

..

   **Example:**

   [OPTIONS]

   UNITS CFS

   HEADLOSS D-W

   QUALITY TRACE Tank23 UNBALANCED CONTINUE 10

[PATTERNS]
==========

   **Purpose:**

   Defines time patterns.

   **Format:**

   One or more lines for each pattern containing:

-  Pattern ID label

-  One or more multipliers

..

   **Remarks:**

   a. Multipliers define how some base quantity (e.g., demand) is
   adjusted for each time period.

a. All patterns share the same time period interval as defined in the
   [TIMES] section.

b. Each pattern can have a different number of time periods.

c. When the simulation time exceeds the pattern length the pattern wraps
   around to its first period.

d. Use as many lines as it takes to include all multipliers for each
   pattern.

..

   **Example:**

   [PATTERNS]

   ;Pattern P1

+----------------+--------+--------+--------+
|    P1 1.1      |    1.4 |    0.9 |    0.7 |
+================+========+========+========+
|    P1 0.6      |    0.5 |    0.8 |    1.0 |
+----------------+--------+--------+--------+
|    ;Pattern P2 |        |        |        |
+----------------+--------+--------+--------+
|    P2 1        |    1   |    1   |    1   |
+----------------+--------+--------+--------+
|    P2 0        |    0   |    1   |        |
+----------------+--------+--------+--------+

[PIPES]
=======

   **Purpose:**

   Defines all pipe links contained in the network.

   **Format:**

   One line for each pipe containing:

-  ID label of pipe

-  ID of start node

-  ID of end node

-  Length, ft (m)

-  Diameter, inches (mm)

-  Roughness coefficient

-  Minor loss coefficient

-  Status (OPEN, CLOSED, or CV)

..

   **Remarks:**

a. Roughness coefficient is unitless for the Hazen-Williams and
   Chezy-Manning head loss formulas and has units of millifeet (mm) for
   the Darcy-Weisbach formula. Choice of head loss formula is supplied
   in the [OPTIONS] section.

b. Setting status to CV means that the pipe contains a check valve
   restricting flow to one direction.

c. If minor loss coefficient is 0 and pipe is OPEN then these two items
   can be dropped form the input line.

..

   **Example:**

   [PIPES]

   ;ID Node1 Node2 Length Diam. Roughness Mloss Status

   ;-------------------------------------------------------------

+-------+-------+--------+------+----+--------+--------+---------+
|    P1 |    J1 |    J2  | 1200 | 12 |    120 |    0.2 |    OPEN |
+=======+=======+========+======+====+========+========+=========+
|    P2 |    J3 |    J2  | 600  | 6  |    110 |    0   |    CV   |
+-------+-------+--------+------+----+--------+--------+---------+
|    P3 |    J1 |    J10 | 1000 | 12 |    120 |        |         |
+-------+-------+--------+------+----+--------+--------+---------+

[PUMPS]
=======

   **Purpose:**

   Defines all pump links contained in the network.

   **Format:**

   One line for each pump containing:

-  ID label of pump

-  ID of start node

-  ID of end node

-  Keyword and Value (can be repeated)

..

   **Remarks:**

a. Keywords consists of:

   -  **POWER** – power value for constant energy pump, hp (kW)

   -  **HEAD** - ID of curve that describes head versus flow for the
      pump

   -  **SPEED** - relative speed setting (normal speed is 1.0, 0 means
      pump is off)

   -  **PATTERN** - ID of time pattern that describes how speed setting
      varies with time

b. Either **POWER** or **HEAD** must be supplied for each pump. The
   other keywords are optional.

..

   **Example:**

   [PUMPS]

   ;ID Node1 Node2 Properties

   ;---------------------------------------------

+----------+---------+--------+--------------------------+
|    Pump1 |    N12  |    N32 |    HEAD Curve1           |
+==========+=========+========+==========================+
|    Pump2 |    N121 |    N55 |    HEAD Curve1 SPEED 1.2 |
+----------+---------+--------+--------------------------+
|    Pump3 |    N22  |    N23 |    POWER 100             |
+----------+---------+--------+--------------------------+

[QUALITY]
=========

   **Purpose:**

   Defines initial water quality at nodes.

   **Format:**

   One line per node containing:

-  Node ID label

-  Initial quality

..

   **Remarks:**

a. Quality is assumed to be zero for nodes not listed.

b. Quality represents concentration for chemicals, hours for water age,
   or percent for source tracing.

c. The [QUALITY] section is optional.

[REACTIONS]
===========

   **Purpose:**

   Defines parameters related to chemical reactions occurring in the
   network.

   **Formats:**

   **ORDER BULK/WALL/TANK** value

   **GLOBAL BULK/WALL** value

   **BULK/WALL/TANK** pipeID value **LIMITING POTENTIAL** value
   **ROUGHNESS CORRELATION** value

   **Definitions:**

   **ORDER** is used to set the order of reactions occurring in the bulk
   fluid, at the pipe wall, or in tanks, respectively. Values for wall
   reactions must be either 0 or 1. If not supplied the default reaction
   order is 1.0.

   **GLOBAL** is used to set a global value for all bulk reaction
   coefficients (pipes and tanks) or for all pipe wall coefficients. The
   default value is zero.

   **BULK, WALL**, and **TANK** are used to override the global reaction
   coefficients for specific pipes and tanks.

   **LIMITING POTENTIAL** specifies that reaction rates are proportional
   to the difference between the current concentration and some limiting
   potential value.

   **ROUGHNESS CORRELATION** will make all default pipe wall reaction
   coefficients be related to pipe roughness in the following manner:

   Head Loss Equation Roughness Correlation Hazen-Williams F / C

   Darcy-Weisbach F / log(e/D)

   Chezy-Manning F*n

   where F = roughness correlation, C = Hazen-Williams C-factor, e =
   Darcy-Weisbach roughness, D = pipe diameter, and n = Chezy-Manning
   roughness coefficient. The default value computed this way can be
   overridden for any pipe by using the **WALL** format to supply a
   specific value for the pipe.

   **Remarks:**

a. Remember to use positive numbers for growth reaction coefficients and
   negative numbers for decay coefficients.

b. The time units for all reaction coefficients are 1/days.

c. All entries in this section are optional. Items offset by slashes (/)
   indicate allowable choices.

..

   **Example:**

   [REACTIONS]

   ORDER WALL 0 ;Wall reactions are zero-order GLOBAL BULK -0.5 ;Global
   bulk decay coeff.

   GLOBAL WALL -1.0 ;Global wall decay coeff. WALL P220 -0.5
   ;Pipe-specific wall coeffs. WALL P244 -0.7

[REPORT]
========

   **Purpose:**

   Describes the contents of the output report produced from a
   simulation.

   **Formats:**

   **PAGESIZE** value

   **FILE** filename

   **STATUS YES/NO/FULL**

   **SUMMARY YES/NO**

   **ENERGY YES/NO**

   **NODES NONE/ALL/**\ node1 node2 ...

   **LINKS NONE/ALL/**\ link1 link2 ...

   parameter **YES/NO**

   parameter **BELOW/ABOVE/PRECISION** value

   **Definitions:**

   **PAGESIZE** sets the number of lines written per page of the output
   report. The default is 0, meaning that no line limit per page is in
   effect.

   **FILE** supplies the name of a file to which the output report will
   be written (ignored by the Windows version of EPANET).

   **STATUS** determines whether a hydraulic status report should be
   generated. If **YES** is selected the report will identify all
   network components that change status during each time step of the
   simulation. If **FULL** is selected, then the status report will also
   include information from each trial of each hydraulic analysis. This
   level of detail is only useful for de-bugging networks that become
   hydraulically unbalanced. The default is **NO**.

   **SUMMARY** determines whether a summary table of number of network
   components and key analysis options is generated. The default is
   **YES**.

   **ENERGY** determines if a table reporting average energy usage and
   cost for each pump is provided. The default is NO.

   **NODES** identifies which nodes will be reported on. You can either
   list individual node ID labels or use the keywords **NONE** or
   **ALL**. Additional **NODES** lines can be used to continue the list.
   The default is **NONE**.

   **LINKS** identifies which links will be reported on. You can either
   list individual link ID labels or use the keywords **NONE** or
   **ALL**. Additional **LINKS** lines can be used to continue the list.
   The default is **NONE**.

   The “parameter” reporting option is used to identify which quantities
   are reported on, how many decimal places are displayed, and what kind
   of filtering should be used to limit output reporting. Node
   parameters that can be reported on include:

-  **Elevation**

-  **Demand**

-  **Head**

-  **Pressure**

-  **Quality.**

..

   Link parameters include:

-  **Length**

-  **Diameter**

-  **Flow**

-  **Velocity**

-  **Headloss**

-  **Position** (same as status – open, active, closed)

-  **Setting** (Roughness for pipes, speed for pumps, pressure/flow
   setting for valves)

-  **Reaction** (reaction rate)

-  **F-Factor** (friction factor).

..

   The default quantities reported are **Demand, Head, Pressure**, and
   **Quality** for nodes and

   **Flow, Velocity**, and **Headloss** for links. The default precision
   is two decimal places.

   **Remarks:**

a. All options assume their default values if not explicitly specified
   in this section.

b. Items offset by slashes (/) indicate allowable choices.

c. The default is to not report on any nodes or links, so a **NODES** or
   **LINKS** option must be supplied if you wish to report results for
   these items.

d. For the Windows version of EPANET, the only [REPORT] option
   recognized is **STATUS**. All others are ignored.

..

   **Example:**

   The following example reports on nodes N1, N2, N3, and N17 and all
   links with velocity above 3.0. The standard node parameters (Demand,
   Head, Pressure, and Quality) are reported on while only Flow,
   Velocity, and F-Factor (friction factor) are displayed for links.

   [REPORT]

   NODES N1 N2 N3 N17 LINKS ALL

   FLOW YES

   VELOCITY PRECISION 4

F. ACTOR PRECISION 4

..

   VELOCITY ABOVE 3.0

[RESERVOIRS]
============

   **Purpose:**

   Defines all reservoir nodes contained in the network.

   **Format:**

   One line for each reservoir containing:

-  ID label

-  Head, ft (m)

-  Head pattern ID (optional)

..

   **Remarks:**

a. Head is the hydraulic head (elevation + pressure head) of water in
   the reservoir.

b. A head pattern can be used to make the reservoir head vary with time.

c. At least one reservoir or tank must be contained in the network.

..

   **Example:**

   [RESERVOIRS]

   ;ID Head Pattern

   ;---------------------

   R1 512 ;Head stays constant R2 120 Pat1 ;Head varies with time

[RULES]
=======

   **Purpose:**

   Defines rule-based controls that modify links based on a combination
   of conditions.

   **Format:**

   Each rule is a series of statements of the form:

   **RULE** ruleID

   **IF** condition_1 **AND** condition_2 **OR** condition_3 **AND**
   condition_4 etc.

   **THEN** action_1 **AND** action_2 etc.

   **ELSE** action_3 **AND** action_4 etc.

   **PRIORITY** value

   where:

+---------------+---+--------------------------------------------------+
|    ruleID     | = |    an ID label assigned to the rule              |
+===============+===+==================================================+
|    conditon_n | = |    a condition clause                            |
+---------------+---+--------------------------------------------------+
|    action_n   | = |    an action clause                              |
+---------------+---+--------------------------------------------------+
|    Priority   | = |    a priority value (e.g., a number from 1 to 5) |
+---------------+---+--------------------------------------------------+

..

   **Condition Clause Format:**

   A condition clause in a Rule-Based Control takes the form of:

object id attribute relation value

   where

   object = a category of network object

   id = the object's ID label

   attribute = an attribute or property of the object

   relation = a relational operator

   value = an attribute value

   Some example conditional clauses are:

   JUNCTION 23 PRESSURE > 20 TANK T200 FILLTIME BELOW 3.5 LINK 44 STATUS
   IS OPEN SYSTEM DEMAND >= 1500

   SYSTEM CLOCKTIME = 7:30 AM

   The Object keyword can be any of the following:

   **NODE LINK SYSTEM**

   **JUNCTION PIPE RESERVOIR PUMP TANK VALVE**

   When **SYSTEM** is used in a condition no ID is supplied.

   The following attributes can be used with Node-type objects:

   **DEMAND HEAD PRESSURE**

   The following attributes can be used with Tanks:

   **LEVEL**

   **FILLTIME** (hours needed to fill a tank)

   **DRAINTIME** (hours needed to empty a tank)

   These attributes can be used with Link-Type objects:

   **FLOW**

   **STATUS** (**OPEN**, **CLOSED**, or **ACTIVE**)

   **SETTING** (pump speed or valve setting)

   The **SYSTEM** object can use the following attributes:

   **DEMAND** (total system demand)

   **TIME** (hours from the start of the simulation expressed either as
   a decimal number or in hours:minutes format)

   **CLOCKTIME** (24-hour clock time with **AM** or **PM** appended)

   Relation operators consist of the following:

   **= IS**

   **<> NOT**

   **< BELOW**

   **> ABOVE**

   **<= >=**

   **Action Clause Format:**

   An action clause in a Rule-Based Control takes the form of:

   object id STATUS/SETTING IS value

+-----------------+-----------------+-----------------+-----------------+
|    where        |                 |                 |                 |
+=================+=================+=================+=================+
|                 |    object       |    =            |    **LINK**,    |
|                 |                 |                 |    **PIPE**,    |
|                 |                 |                 |    **PUMP**, or |
|                 |                 |                 |    **VALVE**    |
|                 |                 |                 |    keyword      |
+-----------------+-----------------+-----------------+-----------------+
|                 |    id           |    =            |    the object's |
|                 |                 |                 |    ID label     |
+-----------------+-----------------+-----------------+-----------------+
|                 |    value        |    =            |    a status     |
|                 |                 |                 |    condition    |
|                 |                 |                 |    (**OPEN** or |
|                 |                 |                 |    **CLOSED**), |
|                 |                 |                 |    pump speed   |
|                 |                 |                 |    setting, or  |
|                 |                 |                 |    valve        |
|                 |                 |                 |    setting      |
+-----------------+-----------------+-----------------+-----------------+

..

   Some example action clauses are:

   LINK 23 STATUS IS CLOSED PUMP P100 SETTING IS 1.5 VALVE 123 SETTING
   IS 90

   **Remarks:**

a. Only the **RULE**, **IF** and **THEN** portions of a rule are
   required; the other portions are optional.

b. When mixing **AND** and **OR** clauses, the **OR** operator has
   higher precedence than **AND**, i.e.,

..

   IF A or B and C

   is equivalent to

   IF (A or B) and C.

If the interpretation was meant to be

   IF A or (B and C)

   then this can be expressed using two rules as in

   IF A THEN ...

   IF B and C THEN ...

c. The **PRIORITY** value is used to determine which rule applies when
   two or more rules require that conflicting actions be taken on a
   link. A rule without a priority value always has a lower priority
   than one with a value. For two rules with the same priority value,
   the rule that appears first is given the higher priority.

..

   **Example:**

   [RULES] RULE 1

   IF TANK 1 LEVEL ABOVE 19.1 THEN PUMP 335 STATUS IS CLOSED AND PIPE
   330 STATUS IS OPEN

   RULE 2

   IF SYSTEM CLOCKTIME >= 8 AM AND SYSTEM CLOCKTIME < 6 PM AND TANK 1
   LEVEL BELOW 12 THEN PUMP 335 STATUS IS OPEN

   RULE 3

   IF SYSTEM CLOCKTIME >= 6 PM OR SYSTEM CLOCKTIME < 8 AM AND TANK 1
   LEVEL BELOW 14 THEN PUMP 335 STATUS IS OPEN

[SOURCES]
=========

   **Purpose:**

   Defines locations of water quality sources.

   **Format:**

   One line for each water quality source containing:

-  Node ID label

-  Source type (**CONCEN, MASS, FLOWPACED**, or **SETPOINT**)

-  Baseline source strength

-  Time pattern ID (optional)

..

   **Remarks:**

a. For **MASS** type sources, strength is measured in mass flow per
   minute. All other types measure source strength in concentration
   units.

b. Source strength can be made to vary over time by specifying a time
   pattern.

c. A **CONCEN** source:

   -  represents the concentration of any external source inflow to the
      node

   -  applies only when the node has a net negative demand (water enters
      the network at the node)

   -  if the node is a junction, reported concentration is the result of
      mixing the source flow and inflow from the rest of the network

   -  if the node is a reservoir, the reported concentration is the
      source concentration

   -  if the node is a tank, the reported concentration is the internal
      concentration of the tank

   -  is best used for nodes that represent source water supplies or
      treatment works (e.g., reservoirs or nodes assigned a negative
      demand)

   -  should not be used at storage tanks with simultaneous
      inflow/outflow.

d. A **MASS, FLOWPACED**, or **SETPOINT** source:

   -  represents a booster source, where the substance is injected
      directly into the network irregardless of what the demand at the
      node is

   -  affects water leaving the node to the rest of the network in the
      following way:

      -  a **MASS** booster adds a fixed mass flow to that resulting
         from inflow to the node

      -  a **FLOWPACED** booster adds a fixed concentration to the
         resultant inflow concentration at the node

      -  a **SETPOINT** booster fixes the concentration of any flow
         leaving the node (as long as the concentration resulting from
         the inflows is below the setpoint)

   -  the reported concentration at a junction or reservoir booster
      source is the concentration that results after the boosting is
      applied; the reported concentration for a tank with a booster
      source is the internal concentration of the tank

   -  is best used to model direct injection of a tracer or disinfectant
      into the network or to model a contaminant intrusion.

e. A [SOURCES] section is not needed for simulating water age or source
   tracing.

..

   **Example:**

   [SOURCES]

   ;Node Type Strength Pattern

   ;--------------------------------

   N1 CONCEN 1.2 Pat1 ;Concentration varies with time N44 MASS 12
   ;Constant mass injection

[STATUS]
========

   **Purpose:**

   Defines initial status of selected links at the start of a
   simulation.

   **Format:**

   One line per link being controlled containing:

-  Link ID label

-  Status or setting

..

   **Remarks:**

a. Links not listed in this section have a default status of **OPEN**
   (for pipes and pumps) or **ACTIVE** (for valves).

b. The status value can be **OPEN** or **CLOSED**. For control valves
   (e.g., PRVs, FCVs, etc.) this means that the valve is either fully
   opened or closed, not active at its control setting.

c. The setting value can be a speed setting for pumps or valve setting
   for valves.

d. The initial status of pipes can also be set in the [PIPES] section.

e. Check valves cannot have their status be preset.

f. Use [CONTROLS] or [RULES] to change status or setting at some future
   point in the simulation.

g. If a **CLOSED** or **OPEN** control valve is to become **ACTIVE**
   again, then its pressure or flow setting must be specified in the
   control or rule that re-activates it.

Example:
~~~~~~~~

   [STATUS]

   ; Link Status/Setting

   ;----------------------

   L22 CLOSED ;Link L22 is closed P14 1.5 ;Speed for pump P14

   PRV1 OPEN ;PRV1 forced open

   ;(overrides normal operation)

[TAGS]
======

   **Purpose:**

   Associates category labels (tags) with specific nodes and links.

   **Format:**

   One line for each node and link with a tag containing

-  the keyword NODE or LINK

-  the node or link ID label

-  the text of the tag label (with no spaces)

..

   **Remarks:**

a. Tags can be useful for assigning nodes to different pressure zones or
   for classifying pipes by material or age.

b. If a node or link’s tag is not identified in this section then it is
   assumed to be blank.

c. The [TAGS] section is optional and has no effect on the hydraulic or
   water quality calculations.

..

   **Example:**

   [TAGS]

   ;Object ID Tag

   ;------------------------------

+---------+------+--------------+
|    NODE | 1001 |    Zone_A    |
+=========+======+==============+
|    NODE | 1002 |    Zone_A    |
+---------+------+--------------+
|    NODE | 45   |    Zone_B    |
+---------+------+--------------+
|    LINK | 201  |    UNCI-1960 |
+---------+------+--------------+
|    LINK | 202  |    PVC-1985  |
+---------+------+--------------+

[TANKS]
=======

   **Purpose:**

   Defines all tank nodes contained in the network.

   **Format:**

   One line for each tank containing:

-  ID label

-  Bottom elevation, ft (m)

-  Initial water level, ft (m)

-  Minimum water level, ft (m)

-  Maximum water level, ft (m)

-  Nominal diameter, ft (m)

-  Minimum volume, cubic ft (cubic meters)

-  Volume curve ID (optional)

..

   **Remarks:**

a. Water surface elevation equals bottom elevation plus water level.

b. Non-cylindrical tanks can be modeled by specifying a curve of volume
   versus water depth in the [CURVES] section.

c. If a volume curve is supplied the diameter value can be any non-zero
   number

d. Minimum volume (tank volume at minimum water level) can be zero for a
   cylindrical tank or if a volume curve is supplied.

e. A network must contain at least one tank or reservoir.

..

   **Example:**

   [TANKS]

   ;ID Elev. InitLvl MinLvl MaxLvl Diam MinVol VolCurve

   ;-----------------------------------------------------------

   ;Cylindrical tank

   T1 100 15 5 25 120 0

   ;Non-cylindrical tank with arbitrary diameter

   T2 100 15 5 25 1 0 VC1

[TIMES]
=======

   **Purpose:**

   Defines various time step parameters used in the simulation.

   **Formats:**

   **DURATION**

   **HYDRAULIC TIMESTEP QUALITY TIMESTEP RULE TIMESTEP PATTERN TIMESTEP
   PATTERN START REPORT TIMESTEP REPORT START**

   **START CLOCKTIME STATISTIC**

   **Definitions:**

   Value (units) Value (units) Value (units) Value (units) Value (units)
   Value (units) Value (units) Value (units) Value (**AM/PM**)

   **NONE/AVERAGED/ MINIMUM/MAXIMUM RANGE**

   **DURATION** is the duration of the simulation. Use 0 to run a single
   period snapshot analysis. The default is 0.

   **HYDRAULIC TIMESTEP** determines how often a new hydraulic state of
   the network is computed. If greater than either the **PATTERN** or
   **REPORT** time step it will be automatically reduced. The default is
   1 hour.

   **QUALITY TIMESTEP** is the time step used to track changes in water
   quality throughout the network. The default is 1/10 of the hydraulic
   time step.

   **RULE TIMESTEP** is the time step used to check for changes in
   system status due to activation of rule-based controls between
   hydraulic time steps. The default is 1/10 of the hydraulic time step.

   **PATTERN TIMESTEP** is the interval between time periods in all time
   patterns. The default is 1 hour.

   **PATTERN START** is the time offset at which all patterns will
   start. For example, a value of 6 hours would start the simulation
   with each pattern in the time period that corresponds to hour 6. The
   default is 0.

   **REPORT TIMESTEP** sets the time interval between which output
   results are reported. The default is 1 hour.

   **REPORT START** is the length of time into the simulation at which
   output results begin to be reported. The default is 0.

   **START CLOCKTIME** is the time of day (e.g., 3:00 PM) at which the
   simulation begins. The default is 12:00 AM midnight.

   **STATISTIC** determines what kind of statistical post-processing
   should be done on the time series of simulation results generated.
   **AVERAGED** reports a set of time-averaged results, **MINIMUM**
   reports only the minimum values, **MAXIMUM** the maximum values, and
   **RANGE** reports the difference between the minimum and maximum
   values. **NONE** reports the full time series for all quantities for
   all nodes and links and is the default.

   **Remarks:**

a. Units can be **SECONDS (SEC), MINUTES (MIN), HOURS**, or **DAYS**.
   The default is hours.

b. If units are not supplied, then time values can be entered as decimal
   hours or in hours:minutes notation.

c. All entries in the [TIMES] section are optional. Items offset by
   slashes (/) indicate allowable choices.

..

   **Example:**

   [TIMES]

   DURATION 240 HOURS QUALITY TIMESTEP 3 MIN REPORT START 120

   STATISTIC AVERAGED START CLOCKTIME 6:00 AM

[TITLE]
=======

   **Purpose:**

   Attaches a descriptive title to the network being analyzed.

   **Format:**

   Any number of lines of text.

   **Remarks:**

   The [TITLE] section is optional.

[VALVES]
========

   **Purpose:**

   Defines all control valve links contained in the network.

   **Format:**

   One line for each valve containing:

-  ID label of valve

-  ID of start node

-  ID of end node

-  Diameter, inches (mm)

-  Valve type

-  Valve setting

-  Minor loss coefficient

..

   **Remarks:**

a. Valve types and settings include:

..

   Valve Type Setting PRV (pressure reducing valve) Pressure, psi (m)

   PSV (pressure sustaining valve) Pressure, psi (m)

   PBV (pressure breaker valve) Pressure, psi (m) FCV (flow control
   valve) Flow (flow units)

   TCV (throttle control valve) Loss Coefficient GPV (general purpose
   valve) ID of head loss curve

b. Shutoff valves and check valves are considered to be part of a pipe,
   not a separate control valve component (see [PIPES])

[VERTICES]
==========

   **Purpose:**

   Assigns interior vertex points to network links.

   **Format:**

   One line for each point in each link containing such points that
   includes:

-  Link ID label

-  X-coordinate

-  Y-coordinate

..

   **Remarks:**

a. Vertex points allow links to be drawn as polylines instead of simple
   straight-lines between their end nodes.

b. The coordinates refer to the same coordinate system used for node and
   label coordinates.

c. A [VERTICES] section is optional and is not used at all when EPANET
   is run as a console application.

..

   **Example:**

   [COORDINATES]

   ;Node X-Coord. Y-Coord

   ;-------------------------------

+------+-------+-----+
|    1 | 10023 | 128 |
+======+=======+=====+
|    2 | 10056 | 95  |
+------+-------+-----+

Report File Format
~~~~~~~~~~~~~~~~~~

   Statements supplied to the [REPORT] section of the input file control
   the contents of the report file generated from a command-line run of
   EPANET. A portion of the report generated from the input file of
   Figure C.1 is shown in Figure C.2. In general a report can contain
   the following sections:

-  Status Section

-  Energy Section

-  Nodes Section

-  Links Section

..

   Status Section

   The Status Section of the output report lists the initial status of
   all reservoirs, tanks, pumps, valves, and closed pipes as well as any
   changes in the status of these components as they occur over time in
   an extended period simulation. The status of reservoirs and tanks
   indicates whether they are filling or emptying. The status of links
   indicates whether they are open or closed and includes the relative
   speed setting for pumps and the pressure/flow setting for control
   valves. To include a Status Section in the report use the command
   **STATUS YES** in the [REPORT] section of the input file.

   Using **STATUS FULL** will also produce a full listing of the
   convergence results for all iterations of each hydraulic analysis
   made during a simulation. This listing will also show which
   components are changing status during the iterations. This level of
   detail is only useful when one is trying to debug a run that fails to
   converge because a component’s status is cycling.

   Energy Section

   The Energy Section of the output report lists overall energy
   consumption and cost for each pump in the network. The items listed
   for each pump include:

-  Percent Utilization (percent of the time that the pump is on-line)

-  Average Efficiency

-  Kilowatt-hours consumed per million gallons (or cubic meters) pumped

-  Average Kilowatts consumed

-  Peak Kilowatts used

-  Average cost per day

..

   Also listed is the total cost per day for pumping and the total
   demand charge (cost based on the peak energy usage) incurred. To
   include an Energy Section in the report the command **ENERGY YES**
   must appear in the [REPORT] section of the input file.

   \*****************************************************************\*

-  E P A N E T \*

-  Hydraulic and Water Quality \*

-  Analysis for Pipe Networks \*

-  Version 2.0 \*

..

   \*****************************************************************\*
   EPANET TUTORIAL

   Input Data File ................... tutorial.inp

   Number of Junctions. 5

   Number of Reservoirs. 1

   Number of Tanks 1

   Number of Pipes 6

   Number of Pumps 1

   Number of Valves 0

   Headloss Formula .................. Hazen-Williams

   Hydraulic Timestep ................ 1.00 hrs

   Hydraulic Accuracy 0.001000

   Maximum Trials 40

   Quality Analysis .................. Chlorine Water Quality Time Step
   ........... 5.00 min Water Quality Tolerance ........... 0.01 mg/L
   Specific Gravity 1.00

   Relative Kinematic Viscosity 1.00

   Relative Chemical Diffusivity 1.00

   Demand Multiplier 1.00

   Total Duration .................... 24.00 hrs Reporting Criteria:

   All Nodes All Links

   Energy Usage:

   ----------------------------------------------------------------

+---------+---------------+----------+------+------+------+
|         | Usage Avg.    |    Kw-hr | Avg. | Peak | Cost |
+=========+===============+==========+======+======+======+
|    Pump | Factor Effic. |    /Mgal | Kw   | Kw   | /day |
+---------+---------------+----------+------+------+------+

..

   ---------------------------------------------------------------- 7
   100.00 75.00 746.34 51.34 51.59 0.00

   ----------------------------------------------------------------

   Demand Charge: 0.00

   Total Cost: 0.00

   **Figure C.2** Excerpt from a Report File (continued on next page)

   Node Results at 0:00 hrs:

   --------------------------------------------------------

   Demand Head Pressure Chlorine

   Node gpm ft psi mg/L

   -------------------------------------------------------- 2 0.00
   893.37 387.10 0.00

   3 325.00 879.78 73.56 0.00

   4 75.00 874.43 75.58 0.00

   5 100.00 872.69 76.99 0.00

   6 75.00 872.71 74.84 0.00

   1 -1048.52 700.00 0.00 1.00 Reservoir

   7 473.52 855.00 2.17 0.00 Tank

   Link Results at 0:00 hrs:

   ----------------------------------------------

Flow Velocity Headloss

   Link gpm fps /1000ft

   ---------------------------------------------- 1 1048.52 2.97 4.53

   2 558.33 1.58 1.41

   3 165.19 1.05 1.07

   4 90.19 0.58 0.35

   5 -9.81 0.06 0.01

   6 473.52 1.93 2.53

   7 1048.52 0.00 -193.37 Pump

   Node Results at 1:00 hrs:

   --------------------------------------------------------

   Demand Head Pressure Chlorine

   Node gpm ft psi mg/L

   -------------------------------------------------------- 2 0.00
   893.92 387.34 1.00

   3 325.00 880.42 73.84 0.99

   4 75.00 875.12 75.88 0.00

   5 100.00 873.40 77.30 0.00

   6 75.00 873.43 75.15 0.00

   1 -1044.60 700.00 0.00 1.00 Reservoir

   7 469.60 855.99 2.59 0.00 Tank

   Link Results at 1:00 hrs:

   ----------------------------------------------

Flow Velocity Headloss

+-----------------+-----------------+-----------------+-----------------+
|    Link         | gpm             | fps             |    /1000ft      |
+=================+=================+=================+=================+
|    ------------ |                 |                 |                 |
| --------------- |                 |                 |                 |
| --------------- |                 |                 |                 |
| ----            |                 |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
|    1            | 1044.60         | 2.96            |    4.50         |
+-----------------+-----------------+-----------------+-----------------+
|    2            | 555.14          | 1.57            |    1.40         |
+-----------------+-----------------+-----------------+-----------------+
|    3            | 164.45          | 1.05            |    1.06         |
+-----------------+-----------------+-----------------+-----------------+
|    4            | 89.45           | 0.57            |    0.34         |
+-----------------+-----------------+-----------------+-----------------+
|    5            | -10.55          | 0.07            |    0.01         |
+-----------------+-----------------+-----------------+-----------------+
|    6            | 469.60          | 1.92            |    2.49         |
+-----------------+-----------------+-----------------+-----------------+
|    7            | 1044.60         | 0.00            |    -193.92 Pump |
+-----------------+-----------------+-----------------+-----------------+

..

   **Figure C.2** Excerpt from a Report File (continued from previous
   page)

   Nodes Section

   The Nodes Section of the output report lists simulation results for
   those nodes and parameters identified in the [REPORT] section of the
   input file. Results are listed for each reporting time step of an
   extended period simulation. The reporting time step is specified in
   the [TIMES] section of the input file. Results at intermediate times
   when certain hydraulic events occur, such as pumps turning on or off
   or tanks closing because they become empty or full, are not reported.

   To have nodal results reported the [REPORT] section of the input file
   must contain the keyword **NODES** followed by a listing of the ID
   labels of the nodes to be included in the report. There can be
   several such **NODES** lines in the file. To report results for all
   nodes use the command **NODES ALL**.

   The default set of reported quantities for nodes includes Demand,
   Head, Pressure, and Water Quality. You can specify how many decimal
   places to use when listing results for a parameter by using commands
   such as **PRESSURE PRECISION 3** in the input file (i.e., use 3
   decimal places when reporting results for pressure). The default
   precision is 2 decimal places for all quantities. You can filter the
   report to list only the occurrences of values below or above a
   certain value by adding statements of the form **PRESSURE BELOW 20**
   to the input file.

   Links Section

   The Links Section of the output report lists simulation results for
   those links and parameters identified in the [REPORT] section of the
   input file. The reporting times follow the same convention as was
   described for nodes in the previous section.

   As with nodes, to have any results for links reported you must
   include the keyword **LINKS** followed by a list of link ID labels in
   the [REPORT] section of the input file. Use the command **LINKS ALL**
   to report results for all links.

   The default parameters reported on for links includes Flow, Velocity,
   and Headloss. Diameter, Length, Water Quality, Status, Setting,
   Reaction Rate, and Friction Factor can be added to these by using
   commands such as **DIAMETER YES** or **DIAMETER PRECISION 0**. The
   same conventions used with node parameters for specifying reporting
   precision and filters also applies to links.

Binary Output File Format
~~~~~~~~~~~~~~~~~~~~~~~~~

   If a third file name is supplied to the command line that runs EPANET
   then the results for all parameters for all nodes and links for all
   reporting time periods will be saved to this file in a special binary
   format. This file can be used for special post- processing purposes.
   Data written to the file are 4-byte integers, 4-byte floats, or
   fixed-size strings whose size is a multiple of 4 bytes. This allows
   the file to be divided conveniently into 4-byte records. The file
   consists of four sections of the following sizes in bytes:

   *Section Size in bytes*

   Prolog 852 + 20*Nnodes + 36*Nlinks + 8*Ntanks

   Energy Use 28*Npumps + 4

   Extended Period (16*Nnodes + 32*Nlinks)*Nperiods Epilog 28

   where

   Nnodes = number of nodes (junctions + reservoirs + tanks) Nlinks =
   number of links (pipes + pumps + valves) Ntanks = number of tanks and
   reservoirs

   Npumps = number of pumps

   Nperiods = number of reporting periods

   and all of these counts are themselves written to the file's Prolog
   or Epilog sections.

   Prolog Section

   The prolog section of the binary Output File contains the following
   data:

+-----------------------+-----------------------+-----------------------+
| **Item**              |    **Type**           |    **Number of        |
|                       |                       |    Bytes**            |
+=======================+=======================+=======================+
| Magic Number ( =      |    Integer            |    4                  |
| 516114521)            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Version (= 200)       |    Integer            |    4                  |
+-----------------------+-----------------------+-----------------------+
| Number of Nodes       |    Integer            |    4                  |
|                       |                       |                       |
| (Junctions +          |                       |                       |
| Reservoirs + Tanks)   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Number of Reservoirs  |    Integer            |    4                  |
| & Tanks               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Number of Links       |    Integer            |    4                  |
|                       |                       |                       |
| (Pipes + Pumps +      |                       |                       |
| Valves)               |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Number of Pumps       |    Integer            |    4                  |
+-----------------------+-----------------------+-----------------------+
| Number of Valves      |    Integer            |    4                  |
+-----------------------+-----------------------+-----------------------+
|    Water Quality      |    Integer            |    4                  |
|    Option 0 = none    |                       |                       |
|                       |                       |                       |
|    1 = chemical       |                       |                       |
|                       |                       |                       |
|    2 = age            |                       |                       |
|                       |                       |                       |
|    3 = source trace   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Index of Node for     |    Integer            |    4                  |
| Source Tracing        |                       |                       |
+-----------------------+-----------------------+-----------------------+
|    Flow Units Option  |    Integer            |    4                  |
|    0 = cfs            |                       |                       |
|                       |                       |                       |
|    1 = gpm            |                       |                       |
|                       |                       |                       |
|    2 = mgd            |                       |                       |
|                       |                       |                       |
|    3 = Imperial mgd 4 |                       |                       |
|    = acre-ft/day      |                       |                       |
|                       |                       |                       |
|    5 = liters/second  |                       |                       |
|                       |                       |                       |
|    6 = liters/minute  |                       |                       |
|                       |                       |                       |
|    7 = megaliters/day |                       |                       |
|                       |                       |                       |
|    8 = cubic          |                       |                       |
|    meters/hour 9 =    |                       |                       |
|    cubic meters/day   |                       |                       |
+-----------------------+-----------------------+-----------------------+

+-----------------------+-----------------------+-----------------------+
|    Pressure Units     |    Integer            |    4                  |
|    Option 0 = psi     |                       |                       |
|                       |                       |                       |
|    1 = meters         |                       |                       |
|                       |                       |                       |
|    2 = kPa            |                       |                       |
+=======================+=======================+=======================+
| Statistics Flag       |    Integer            |    4                  |
|                       |                       |                       |
|    0 = no statistical |                       |                       |
|    processing 1 =     |                       |                       |
|    results are        |                       |                       |
|    time-averaged      |                       |                       |
|                       |                       |                       |
|    2 = only minimum   |                       |                       |
|    values reported    |                       |                       |
|                       |                       |                       |
|    3 = only maximum   |                       |                       |
|    values reported 4  |                       |                       |
|    = only ranges      |                       |                       |
|    reported           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Reporting Start Time  |    Integer            |    4                  |
| (seconds)             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Reporting Time Step   |    Integer            |    4                  |
| (seconds)             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Simulation Duration   |    Integer            |    4                  |
| (seconds)             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Problem Title (1st    |    Char               |    80                 |
| line)                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Problem Title (2nd    |    Char               |    80                 |
| line)                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Problem Title (3rd    |    Char               |    80                 |
| line)                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Name of Input File    |    Char               |    260                |
+-----------------------+-----------------------+-----------------------+
| Name of Report File   |    Char               |    260                |
+-----------------------+-----------------------+-----------------------+
| Name of Chemical      |    Char               |    16                 |
+-----------------------+-----------------------+-----------------------+
| Chemical              |    Char               |    16                 |
| Concentration Units   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| ID Label of Each Node |    Char               |    16                 |
+-----------------------+-----------------------+-----------------------+
| ID Label of Each Link |    Char               |    16                 |
+-----------------------+-----------------------+-----------------------+
| Index of Start Node   |    Integer            |    4*Nlinks           |
| of Each Link          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Index of End Node of  |    Integer            |    4*Nlinks           |
| Each Link             |                       |                       |
+-----------------------+-----------------------+-----------------------+
|    Type Code of Each  |    Integer            |    4*Nlinks           |
|    Link 0 = Pipe with |                       |                       |
|    CV                 |                       |                       |
|                       |                       |                       |
|    1 = Pipe           |                       |                       |
|                       |                       |                       |
|    2 = Pump           |                       |                       |
|                       |                       |                       |
|    3 = PRV            |                       |                       |
|                       |                       |                       |
|    4 = PSV            |                       |                       |
|                       |                       |                       |
|    5 = PBV            |                       |                       |
|                       |                       |                       |
|    6 = FCV            |                       |                       |
|                       |                       |                       |
|    7 = TCV            |                       |                       |
|                       |                       |                       |
|    8 = GPV            |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Node Index of Each    |    Integer            |    4*Ntanks           |
| Tank                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Cross-Sectional Area  |    Float              |    4*Ntanks           |
| of Each Tank          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Elevation of Each     |    Float              |    4*Nnodes           |
| Node                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Length of Each Link   |    Float              |    4*Nlinks           |
+-----------------------+-----------------------+-----------------------+
| Diameter of Each Link |    Float              |    4*Nlinks           |
+-----------------------+-----------------------+-----------------------+

..

   There is a one-to-one correspondence between the order in which the
   ID labels for nodes and links are written to the file and the index
   numbers of these components. Also, reservoirs are distinguished from
   tanks by having their cross-sectional area set to zero.

   Energy Use Section

   The Energy Use section of the binary output file immediately follows
   the Prolog section. It contains the following data:

+-----------------------+-----------------------+-----------------------+
| **Item**              |    **Type**           |    **Number of        |
|                       |                       |    Bytes**            |
+=======================+=======================+=======================+
| Repeated for each     |                       |                       |
| pump:                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| -  Pump Index in List |    Float              |    4                  |
|    of Links           |                       |                       |
+-----------------------+-----------------------+-----------------------+
| -  Pump Utilization   |    Float              |    4                  |
|    (%)                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| -  Average Efficiency |    Float Float        |    4                  |
|    (%)                |                       |                       |
|                       |                       |    4                  |
| -  Average            |                       |                       |
|    Kwatts/Million     |                       |                       |
|    Gallons            |                       |                       |
|    (/Meter:sup:`3`)   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| -  Average Kwatts     |    Float              |    4                  |
+-----------------------+-----------------------+-----------------------+
| -  Peak Kwatts        |    Float              |    4                  |
+-----------------------+-----------------------+-----------------------+
| -  Average Cost Per   |    Float              |    4                  |
|    Day                |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Overall Peak Energy   |    Float              |    4                  |
| Usage                 |                       |                       |
+-----------------------+-----------------------+-----------------------+

..

   The statistics reported in this section refer to the period of time
   between the start of the output reporting period and the end of the
   simulation.

   Extended Period Section

   The Extended Period section of the binary Output File contains
   simulation results for each reporting period of an analysis (the
   reporting start time and time step are written to the Output File's
   Prolog section and the number of steps is written to the Epilog
   section). For each reporting period the following values are written
   to the file:

+-----------------------+-----------------------+-----------------------+
| **Item**              |    **Type**           |    **Size in Bytes**  |
+=======================+=======================+=======================+
| Demand at Each Node   |    Float              |    4*Nnodes           |
+-----------------------+-----------------------+-----------------------+
| Hydraulic Head at     |    Float              |    4*Nnodes           |
| Each Node             |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Pressure at Each Node |    Float              |    4*Nnodes           |
+-----------------------+-----------------------+-----------------------+
| Water Quality at Each |    Float              |    4*Nnodes           |
| Node                  |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Flow in Each Link     |    Float              |    4*Nlinks           |
|                       |                       |                       |
| (negative for reverse |                       |                       |
| flow)                 |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Velocity in Each Link |    Float              |    4*Nlinks           |
+-----------------------+-----------------------+-----------------------+
| Headloss per 1000     |    Float              |    4*Nlinks           |
| Units of Length for   |                       |                       |
| Each Link (Negative   |                       |                       |
| of head gain for      |                       |                       |
| pumps and total head  |                       |                       |
|                       |                       |                       |
| loss for valves)      |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Average Water Quality |    Float              |    4*Nlinks           |
| in Each Link          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Status Code for Each  |    Float              |    4*Nlinks           |
| Link                  |                       |                       |
|                       |                       |                       |
|    0 = closed (max.   |                       |                       |
|    head exceeded) 1 = |                       |                       |
|    temporarily closed |                       |                       |
|                       |                       |                       |
|    2 = closed         |                       |                       |
|                       |                       |                       |
|    3 = open           |                       |                       |
|                       |                       |                       |
|    4 = active         |                       |                       |
|    (partially open)   |                       |                       |
|                       |                       |                       |
|    5 = open (max.     |                       |                       |
|    flow exceeded) 6 = |                       |                       |
|    open (flow setting |                       |                       |
|    not met)           |                       |                       |
|                       |                       |                       |
|    7 = open (pressure |                       |                       |
|    setting not met)   |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Setting for Each      |    Float              |    4*Nlinks           |
| Link:                 |                       |                       |
|                       |                       |                       |
|    Roughness          |                       |                       |
|    Coefficient for    |                       |                       |
|    Pipes Speed for    |                       |                       |
|    Pumps              |                       |                       |
|                       |                       |                       |
|    Setting for Valves |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Reaction Rate for     |    Float              |    4*Nlinks           |
| Each Link             |                       |                       |
| (mass/L/day)          |                       |                       |
+-----------------------+-----------------------+-----------------------+
| Friction Factor for   |    Float              |    4*Nlinks           |
| Each Link             |                       |                       |
+-----------------------+-----------------------+-----------------------+

..

   Epilog Section

   The Epilog section of the binary output file contains the following
   data:

+--------------------------------------+-------------+------------------------+
| **Item**                             |    **Type** |    **Number of Bytes** |
+======================================+=============+========================+
| Average bulk reaction rate (mass/hr) |    Float    |    4                   |
+--------------------------------------+-------------+------------------------+
| Average wall reaction rate (mass/hr) |    Float    |    4                   |
+--------------------------------------+-------------+------------------------+
| Average tank reaction rate (mass/hr) |    Float    |    4                   |
+--------------------------------------+-------------+------------------------+
| Average source inflow rate (mass/hr) |    Float    |    4                   |
+--------------------------------------+-------------+------------------------+
| Number of Reporting Periods          |    Integer  |    4                   |
+--------------------------------------+-------------+------------------------+
| Warning Flag:                        |    Integer  |    4                   |
|                                      |             |                        |
|    0 = no warnings                   |             |                        |
|                                      |             |                        |
|    1 = warnings were generated       |             |                        |
+--------------------------------------+-------------+------------------------+
| Magic Number ( = 516114521)          |    Integer  |    4                   |
+--------------------------------------+-------------+------------------------+

..

   The mass units of the reaction rates both here and in the Extended
   Period output depend on the concentration units assigned to the
   chemical being modeled. The reaction rates listed in this section
   refer to the average of the rates seen in all pipes (or all tanks)
   over the entire reporting period of the simulation.

   (This page intentionally left blank.)

D. ANALYSIS ALGORITHMS
======================

Hydraulics
~~~~~~~~~~

   The method used in EPANET to solve the flow continuity and headloss
   equations that characterize the hydraulic state of the pipe network
   at a given point in time can be termed a hybrid node-loop approach.
   Todini and Pilati (1987) and later Salgado et al. (1988) chose to
   call it the "Gradient Method". Similar approaches have been described
   by Hamam and Brameller (1971) (the "Hybrid Method) and by Osiadacz
   (1987) (the "Newton Loop-Node Method"). The only difference between
   these methods is the way in which link flows are updated after a new
   trial solution for nodal heads has been found. Because Todini's
   approach is simpler, it was chosen for use in EPANET.

   Assume we have a pipe network with N junction nodes and NF fixed
   grade nodes (tanks and reservoirs). Let the flow-headloss relation in
   a pipe between nodes i and j be given as:

   *H* − *H* = *h* = *rQ n* + *mQ* 2

   D.1

   *i j ij ij ij*

   where *H* = nodal head, *h* = headloss, *r* = resistance coefficient,
   *Q* = flow rate, *n* = flow exponent, and *m* = minor loss
   coefficient. The value of the resistance coefficient will depend on
   which friction headloss formula is being used (see below). For pumps,
   the headloss (negative of the head gain) can be represented by a
   power law of the form

*hij*

   = −\ *ψ* 2 (*h*

-  *
   r* (*Qij*

..

   / *ψ* ) *n* )

   where *h\ 0* is the shutoff head for the pump, ω is a relative speed
   setting, and r and n are the pump curve coefficients. The second set
   of equations that must be satisfied is flow continuity around all
   nodes:

   ∑\ *Qij* − *Di* = 0

   *j*

   for i = 1,... N. D.2

   where *D\ i* is the flow demand at node i and by convention, flow
   into a node is positive. For a set of known heads at the fixed grade
   nodes, we seek a solution for all heads *H\ i* and flows *Q\ ij* that
   satisfy Eqs. (D.1) and (D.2).

   The Gradient solution method begins with an initial estimate of flows
   in each pipe that may not necessarily satisfy flow continuity. At
   each iteration of the method, new nodal heads are found by solving
   the matrix equation:

**AH** = **F**

   D.3

   where **A** = an (NxN) Jacobian matrix, **H** = an (Nx1) vector of
   unknown nodal heads, and **F** = an (Nx1) vector of right hand side
   terms

   The diagonal elements of the Jacobian matrix are:

*Aii*

   = ∑ *pij*

*j*

   while the non-zero, off-diagonal terms are:

*Aij*

   = − *pij*

   where *p\ ij* is the inverse derivative of the headloss in the link
   between nodes i and j with respect to flow. For pipes,

*pij*

   =

   *nr Qij*

   1

   *n*\ −1 + 2\ *m Q*

   while for pumps

*pij*

   = 1

   *nψ* 2 *r*\ (*Q*

   / *ψ* )\ *n*\ −1

   Each right hand side term consists of the net flow imbalance at a
   node plus a flow correction factor:

    

   *Fi* =  ∑\ *Qij* − *Di*  + ∑ *yij* + ∑ *pif H f*

    *j*  *j f*

   where the last term applies to any links connecting node i to a fixed
   grade node f and the flow correction factor *y\ ij* is:

*yij*

   = *p* (*r Q n* + *m Q* 2 )sgn(\ *Q* )

   for pipes and

*yij*

   = − *p ψ* 2 (*h* − *r*\ (*Q* / *ψ* ) *n* )

   for pumps, where sgn(x) is 1 if x > 0 and -1 otherwise. (*Q\ ij* is
   always positive for pumps.)

   After new heads are computed by solving Eq. (D.3), new flows are
   found from:

*Qij*

   = *Q* − (*y* − *p* (*H* − *H* ))

   D.4

   If the sum of absolute flow changes relative to the total flow in all
   links is larger than some tolerance (e.g., 0.001), then Eqs. (D.3)
   and (D.4) are solved once again. The flow update formula (D.4) always
   results in flow continuity around each node after the first
   iteration.

   EPANET implements this method using the following steps:

1. The linear system of equations D.3 is solved using a sparse matrix
   method based on node re-ordering (George and Liu, 1981). After re-
   ordering the nodes to minimize the amount of fill-in for matrix A, a
   symbolic factorization is carried out so that only the non-zero
   elements of A need be stored and operated on in memory. For extended
   period simulation this re-ordering and factorization is only carried
   out once at the start of the analysis.

2. For the very first iteration, the flow in a pipe is chosen equal to
   the flow corresponding to a velocity of 1 ft/sec, while the flow
   through a pump equals the design flow specified for the pump. (All
   computations are made with head in feet and flow in cfs).

3. The resistance coefficient for a pipe (*r*) is computed as described
   in Table 3.1. For the Darcy-Weisbach headloss equation, the friction
   factor *f* is computed by different equations depending on the flow’s
   Reynolds Number (Re):

..

   Hagen – Poiseuille formula for Re < 2,000 (Bhave, 1991):

   *f* = 64

Re
--

   Swamee and Jain approximation to the Colebrook - White equation for
   Re > 4,000 (Bhave, 1991):

*f* =

  *σ*
-------

   0.25

5.74  2

   \ *Ln*\  3.7\ *d* + Re0.9 

  
------

   Cubic Interpolation From Moody Diagram for 2,000 < Re < 4,000
   (Dunlop, 1991):

   *f* = ( *X*\ 1 + *R*\ ( *X* 2 + *R*\ ( *X* 3 + *X* 4)))

*R* = Re 2000
-------------

   *X* 1 = 7\ *FA* − *FB*

   *X* 2 = 0.128 − 17\ *FA* + 2.5\ *FB X* 3 = −0.128 + 13\ *FA* −
   2\ *FB*

   *X* 4 = *R*\ (0.032 − 3\ *FA* + 0.5\ *FB*)

   *FA* = (*Y* 3)−2

.. _section-1:


-

   *FB* = *FA*\  2 −

.. _section-2:


-

   0.00514215 

   (*Y* 2)(\ *Y* 3) 

*Y* 2 =

   *σ*

   3.7\ *d*

.. _section-3:

+ 5.74
------

   Re0.9

   *Y* 3 = −0.86859\ *Ln*\  *σ*

.. _section-4:

+ 5.74 
--------

    3.7\ *d* 40000.9 

   where *σ* = pipe roughness and *d* = pipe diameter.

4. The minor loss coefficient based on velocity head (*K*) is converted
   to one based on flow (*m*) with the following relation:

..

   *m* = 0.02517\ *K*

   *d* 4

5. Emitters at junctions are modeled as a fictitious pipe between the
   junction and a fictitious reservoir. The pipe’s headloss parameters
   are *n* = (1/γ), *r* = (1/C):sup:`n`, and *m* = 0 where C is the
   emitter’s discharge coefficient and γ is its pressure exponent. The
   head at the fictitious reservoir is the elevation of the junction.
   The computed flow through the fictitious pipe becomes the flow
   associated with the emitter.

6. Open valves are assigned an *r*-value by assuming the open valve acts
   as a smooth pipe (f = 0.02) whose length is twice the valve diameter.
   Closed links are assumed to obey a linear headloss relation with a
   large resistance factor, i.e., *h* = 10\ :sup:`8`\ *Q*, so that *p* =
   10\ :sup:`-8` and *y* = *Q*. For links where *(r+m)Q* <
   10\ :sup:`-7`, *p* = 10\ :sup:`7` and *y = Q/n*.

7. Status checks on pumps, check valves (CVs), flow control valves, and
   pipes connected to full/empty tanks are made after every other
   iteration, up until the 10th iteration. After this, status checks are
   made only after convergence is achieved. Status checks on pressure
   control valves (PRVs and PSVs) are made after each iteration.

8. During status checks, pumps are closed if the head gain is greater
   than the shutoff head (to prevent reverse flow). Similarly, check
   valves are closed if the headloss through them is negative (see
   below). When these conditions are not present, the link is re-opened.
   A similar status check is made for links connected to empty/full
   tanks. Such links are closed if the difference in head across the
   link would cause an empty tank to drain or a full tank to fill. They
   are re- opened at the next status check if such conditions no longer
   hold.

9. Simply checking if *h* < 0 to determine if a check valve should be
   closed or open was found to cause cycling between these two states in
   some networks due to limits on numerical precision. The following
   procedure was devised to provide a more robust test of the status of
   a check valve (CV):

..

   if \|\ *h*\ \| > Htol then

   if *h* < -Htol then status = CLOSED if *Q* < -Qtol then status =
   CLOSED else status = OPEN

   else

   if *Q* < -Qtol then status = CLOSED

   else status = unchanged where Htol = 0.0005 ft and Qtol = 0.001 cfs.

10. If the status check closes an open pump, pipe, or CV, its flow is
    set to 10\ :sup:`-6` cfs. If a pump is re-opened, its flow is
    computed by applying the current head gain to its characteristic
    curve. If a pipe or CV is re- opened, its flow is determined by
    solving Eq. (D.1) for *Q* under the current headloss *h*, ignoring
    any minor losses.

11. Matrix coefficients for pressure breaker valves (PBVs) are set to
    the following: *p* = 10\ :sup:`8` and *y* = 10\ :sup:`8`\ Hset,
    where Hset is the pressure drop setting for the valve (in feet).
    Throttle control valves (TCVs) are treated as pipes with *r* as
    described in item 6 above and *m* taken as the converted value of
    the valve setting (see item 4 above).

12. Matrix coefficients for pressure reducing, pressure sustaining, and
    flow control valves (PRVs, PSVs, and FCVs) are computed after all
    other links have been analyzed. Status checks on PRVs and PSVs are
    made as described in item 7 above. These valves can either be
    completely open, completely closed, or active at their pressure or
    flow setting.

13. The logic used to test the status of a PRV is as follows:

..

   If current status = ACTIVE then

   if Q < -Qtol then new status = CLOSED if Hi < Hset + Hml – Htol then
   new status = OPEN

   else new status = ACTIVE

   If curent status = OPEN then

   if Q < -Qtol then new status = CLOSED if Hi > Hset + Hml + Htol then
   new status = ACTIVE

   else new status = OPEN

   If current status = CLOSED then if Hi > Hj + Htol

   and Hi < Hset – Htol then new status = OPEN if Hi > Hj + Htol

   and Hj < Hset - Htol then new status = ACTIVE

   else new status = CLOSED

   where Q is the current flow through the valve, Hi is its upstream
   head, Hj is its downstream head, Hset is its pressure setting
   converted to head, Hml is the minor loss when the valve is open (=
   mQ\ :sup:`2`), and Htol and Qtol are the same values used for check
   valves in

   item 9 above. A similar set of tests is used for PSVs, except that
   when testing against Hset, the i and j subscripts are switched as are
   the > and < operators.

14. Flow through an active PRV is maintained to force continuity at its
    downstream node while flow through a PSV does the same at its
    upstream node. For an active PRV from node i to j:

..

   *p\ ij* = 0

   *F\ j* = *F\ j* + 10\ :sup:`8`\ Hset

   *A\ jj* = *A\ jj* + 10\ :sup:`8`

   This forces the head at the downstream node to be at the valve
   setting Hset. An equivalent assignment of coefficients is made for an
   active PSV except the subscript for F and A is the upstream node i.
   Coefficients for open/closed PRVs and PSVs are handled in the same
   way as for pipes.

15. For an active FCV from node i to j with flow setting Qset, Qset is
    added to the flow leaving node i and entering node j, and is
    subtracted from *F\ i* and added to *F\ j*. If the head at node i is
    less than that at node j, then the valve cannot deliver the flow and
    it is treated as an open pipe.

16. After initial convergence is achieved (flow convergence plus no
    change in status for PRVs and PSVs), another status check on pumps,
    CVs, FCVs, and links to tanks is made. Also, the status of links
    controlled by pressure switches (e.g., a pump controlled by the
    pressure at a junction node) is checked. If any status change
    occurs, the iterations must continue for at least two more
    iterations (i.e., a convergence check is skipped on the very next
    iteration). Otherwise, a final solution has been obtained.

17. For extended period simulation (EPS), the following procedure is
    implemented:

    a. After a solution is found for the current time period, the time
       step for the next solution is the minimum of:

       -  the time until a new demand period begins,

       -  the shortest time for a tank to fill or drain,

       -  the shortest time until a tank level reaches a point that
          triggers a change in status for some link (e.g., opens or
          closes a pump) as stipulated in a simple control,

       -  the next time until a simple timer control on a link kicks in,

       -  the next time at which a rule-based control causes a status
          change somewhere in the network.

..

   In computing the times based on tank levels, the latter are assumed
   to change in a linear fashion based on the current flow solution. The
   activation time of rule-based controls is computed as follows:

-  Starting at the current time, rules are evaluated at a rule time
   step. Its default value is 1/10 of the normal hydraulic time step
   (e.g., if hydraulics are updated every hour, then rules are evaluated
   every 6 minutes).

-  Over this rule time step, clock time is updated, as are the water
   levels in storage tanks (based on the last set of pipe flows
   computed).

-  If a rule's conditions are satisfied, then its actions are added to a
   list. If an action conflicts with one for the same link already on
   the list then the action from the rule with the higher priority stays
   on the list and the other is removed. If the priorities are the same
   then the original action stays on the list.

-  After all rules are evaluated, if the list is not empty then the new
   actions are taken. If this causes the status of one or more links to
   change then a new hydraulic solution is computed and the process
   begins anew.

-  If no status changes were called for, the action list is cleared and
   the next rule time step is taken unless the normal hydraulic time
   step has elapsed.

b. Time is advanced by the computed time step, new demands are found,
   tank levels are adjusted based on the current flow solution, and link
   control rules are checked to determine which links change status.

c. A new set of iterations with Eqs. (D.3) and (D.4) are begun at the
   current set of flows.

Water Quality
~~~~~~~~~~~~~

   The governing equations for EPANET’s water quality solver are based
   on the principles of conservation of mass coupled with reaction
   kinetics. The following phenomena are represented (Rossman et al.,
   1993; Rossman and Boulos, 1996):

   Advective Transport in Pipes

   A dissolved substance will travel down the length of a pipe with the
   same average velocity as the carrier fluid while at the same time
   reacting (either growing or decaying) at some given rate.
   Longitudinal dispersion is usually not an important transport
   mechanism under most operating conditions. This means there is no
   intermixing of mass between adjacent parcels of water traveling down
   a pipe. Advective transport within a pipe is represented with the
   following equation:

   ∂ *Ci = - u* ∂ *Ci + r( C )* D.5

   ∂\ *t i* ∂\ *x i*

   where *C\ i* = concentration (mass/volume) in pipe i as a function of
   distance x and time t, *u\ i* = flow velocity (length/time) in pipe
   i, and *r* = rate of reaction (mass/volume/time) as a function of
   concentration.

   Mixing at Pipe Junctions

   At junctions receiving inflow from two or more pipes, the mixing of
   fluid is taken to be complete and instantaneous. Thus the
   concentration of a substance in water leaving the junction is simply
   the flow-weighted sum of the concentrations from the inflowing pipes.
   For a specific node k one can write:

   ∑ *jσ I Q j C j*\ \|\ *x* = *L j*

   *C =*

   *+ Qk* ,\ *ext Ck* ,\ *ext*

   D.6

*i*\ \|\ *x* = 0

∑ *jσ I k*

*Q j + Qk* ,\ *ext*

   where i = link with flow leaving node k, *I\ k* = set of links with
   flow into k, *L\ j* = length of link j, *Q\ j* = flow (volume/time)
   in link j, *Q\ k,ext* = external source flow entering the network at
   node k, and *C\ k,ext* = concentration of the external flow entering
   at node k. The notation *C\ i|x=0* represents the concentration at
   the start of link i, while *C\ i|x=L* is the concentration at the end
   of the link.

   Mixing in Storage Facilities

   It is convenient to assume that the contents of storage facilities
   (tanks and reservoirs) are completely mixed. This is a reasonable
   assumption for many tanks operating under fill-and-draw conditions
   providing that sufficient momentum flux is imparted to the inflow
   (Rossman and Grayman, 1999). Under completely mixed conditions the
   concentration throughout the tank is a blend of the current contents
   and that of any entering water. At the same time, the internal
   concentration could be changing due to reactions. The following
   equation expresses these phenomena:

   ∂\ (V s Cs )

∂\ *t =*

   ∑\ *iσ I s*

*Qi Ci*\ \|\ *x* = *Li -*

   ∑ *jσ O\ s*

*Q j Cs + r*\ (*Cs* ) D.7

   where *V\ s* = volume in storage at time t, *C\ s* = concentration
   within the storage facility, *I\ s* = set of links providing flow
   into the facility, and *O\ s* = set of links withdrawing flow from
   the facility.

   Bulk Flow Reactions

   While a substance moves down a pipe or resides in storage it can
   undergo reaction with constituents in the water column. The rate of
   reaction can generally be described as a power function of
   concentration:

   *r* = *kCn*

   where *k* = a reaction constant and *n* = the reaction order. When a
   limiting concentration exists on the ultimate growth or loss of a
   substance then the rate expression becomes

   *R* = *K* (*C* − *C*)\ *C* (*n*\ −1)

   *R* = *K* (*C* − *C* )\ *C* (*n*\ −1)

   for *n* > 0, *K\ b* > 0 for *n* > 0, *K\ b* < 0

   where *C\ L* = the limiting concentration.

   Some examples of different reaction rate expressions are:

-  *Simple First-Order Decay (CL = 0, K\ b < 0, n = 1)*

..

   *R* = *Kb C*

   The decay of many substances, such as chlorine, can be modeled
   adequately as a simple first-order reaction.

-  *First-Order Saturation Growth (CL > 0, K\ b > 0, n = 1):*

..

   *R* = *Kb* (*CL* − *C*)

   This model can be applied to the growth of disinfection by-products,
   such as trihalomethanes, where the ultimate formation of by-product
   (*C\ L*) is limited by the amount of reactive precursor present.

-  *Two-Component, Second Order Decay (CL* ≠ *0, K\ b < 0, n = 2):*

..

   *R* = *Kb C*\ (*C* − *CL* )

   This model assumes that substance A reacts with substance B in some
   unknown ratio to produce a product P. The rate of disappearance of A
   is proportional to the product of A and B remaining. *C\ L* can be
   either positive or negative, depending on whether either component A
   or B is in excess, respectively. Clark (1998) has had success in
   applying this model to chlorine decay data that did not conform to
   the simple first-order model.

-  *Michaelis-Menton Decay Kinetics (CL > 0, K\ b < 0, n < 0):*

..

   *R* = *Kb C*

   *CL* − *C*

   As a special case, when a negative reaction order *n* is specified,
   EPANET will utilize the Michaelis-Menton rate equation, shown above
   for a decay reaction. (For growth reactions the denominator becomes
   *C\ L + C*.) This rate equation is often used to describe
   enzyme-catalyzed reactions and microbial growth. It produces first-
   order behavior at low concentrations and zero-order behavior at
   higher concentrations. Note that for decay reactions, *C\ L* must be
   set higher than the initial concentration present.

   Koechling (1998) has applied Michaelis-Menton kinetics to model
   chlorine decay in a number of different waters and found that both
   *K\ b* and *C\ L* could be related to the water’s organic content and
   its ultraviolet absorbance as follows:

   *K* = −0.32\ *UVA*\ 1.365 (100*UVA*)

*b DOC*

   *CL* = 4.98\ *UVA* − 1.91\ *DOC*

   where UVA = ultraviolet absorbance at 254 nm (1/cm) and DOC =
   dissolved organic carbon concentration (mg/L).

Note: These expressions apply only for values of *K\ b* and *C\ L* used with Michaelis-Menton kinetics.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  *Zero-Order growth (CL = 0, K\ b = 1, n = 0) R = 1.0*

..

   This special case can be used to model water age, where with each
   unit of time the “concentration” (i.e., age) increases by one unit.

   The relationship between the bulk rate constant seen at one
   temperature (T1) to that at another temperature (T2) is often
   expressed using a van’t Hoff - Arrehnius equation of the form:

*Kb* 2

   *T* 2−\ *T* 1

   *b*\ 1

   where θ is a constant. In one investigation for chlorine, θ was
   estimated to be 1.1 when T1 was 20 deg. C (Koechling, 1998).

   Pipe Wall Reactions

   While flowing through pipes, dissolved substances can be transported
   to the pipe wall and react with material such as corrosion products
   or biofilm that are on or close to the wall. The amount of wall area
   available for reaction and the rate of mass transfer between the bulk
   fluid and the wall will also influence the overall rate of this
   reaction. The surface area per unit volume, which for a pipe equals 2
   divided by the radius, determines the former factor. The latter
   factor can be represented by a mass transfer coefficient whose value
   depends on the molecular diffusivity of the reactive species and on
   the Reynolds number of the flow (Rossman et. al, 1994). For first-
   order kinetics, the rate of a pipe wall reaction can be expressed as:

   *r* = 2\ *kw k f C*

   *R*\ (*kw* + *k f* )

   where *k\ w* = wall reaction rate constant (length/time), *k\ f* =
   mass transfer coefficient (length/time), and R = pipe radius. For
   zero-order kinetics the reaction rate cannot be any higher than the
   rate of mass transfer, so

   *r* = *MIN* (*kw* , *k f C*)(2 / *R*)

   where *k\ w* now has units of mass/area/time.

   Mass transfer coefficients are usually expressed in terms of a
   dimensionless Sherwood number (*Sh*):

*k* = *Sh D*

*f d*

   in which *D* = the molecular diffusivity of the species being
   transported (length:sup:`2`/time) and *d* = pipe diameter. In fully
   developed laminar flow, the average Sherwood number along the length
   of a pipe can be expressed as

   *Sh* = 3.65 +

0.0668(\ *d* / *L*) Re *Sc*

1 + 0.04[(\ *d* / *L*) Re *Sc*]2 / 3

   in which *Re* = Reynolds number and *Sc* = Schmidt number (kinematic
   viscosity of water divided by the diffusivity of the chemical)
   (Edwards et.al, 1976). For turbulent flow the empirical correlation
   of Notter and Sleicher (1971) can be used:

   *Sh* = 0.0149 Re0.88 *Sc*\ 1/ 3

   System of Equations

   When applied to a network as a whole, Equations D.5-D.7 represent a
   coupled set of differential/algebraic equations with time-varying
   coefficients that must be solved for *C\ i* in each pipe i and *C\ s*
   in each storage facility s. This solution is subject to the following
   set of externally imposed conditions:

-  initial conditions that specify *C\ i* for all x in each pipe i and
   *C\ s* in each storage facility s at time 0,

-  boundary conditions that specify values for *C\ k,ext* and *Q\ k,ext*
   for all time t at each node k which has external mass inputs

-  hydraulic conditions which specify the volume *V\ s* in each storage
   facility s and the flow *Q\ i* in each link i at all times t.

..

   Lagrangian Transport Algorithm

   EPANET’s water quality simulator uses a Lagrangian time-based
   approach to track the fate of discrete parcels of water as they move
   along pipes and mix together at junctions between fixed-length time
   steps (Liou and Kroon, 1987). These water quality time steps are
   typically much shorter than the hydraulic time step (e.g., minutes
   rather than hours) to accommodate the short times of travel that can
   occur within pipes. As time progresses, the size of the most upstream
   segment in a pipe increases as water enters the pipe while an equal
   loss in size of the most downstream segment occurs as water leaves
   the link. The size of the segments in between these remains
   unchanged. (See Figure D.1).

   The following steps occur at the end of each such time step:

1. The water quality in each segment is updated to reflect any reaction
   that may have occurred over the time step.

2. The water from the leading segments of pipes with flow into each
   junction is blended together to compute a new water quality value at
   the junction. The volume contributed from each segment equals the
   product of its pipe’s flow rate and the time step. If this volume
   exceeds that of the segment then the segment is destroyed and the
   next one in line behind it begins to contribute its volume.

3. Contributions from outside sources are added to the quality values at
   the junctions. The quality in storage tanks is updated depending on
   the method used to model mixing in the tank (see below).

4. New segments are created in pipes with flow out of each junction,
   reservoir, and tank. The segment volume equals the product of the
   pipe flow and the time step. The segment’s water quality equals the
   new quality value computed for the node.

..

   To cut down on the number of segments, Step 4 is only carried out if
   the new node quality differs by a user-specified tolerance from that
   of the last segment in the outflow pipe. If the difference in quality
   is below the tolerance then the size of the current last segment in
   the outflow pipe is simply increased by the volume flowing into the
   pipe over the time step.

   This process is then repeated for the next water-quality time step.
   At the start of the next hydraulic time step the order of segments in
   any links that experience a flow reversal is switched. Initially each
   pipe in the network consists of a single segment whose quality equals
   the initial quality assigned to the upstream node.

   |image147|

|image148|

   **Figure D.1** Behavior of Segments in the Lagrangian Solution Method

References
~~~~~~~~~~

   Bhave, P.R. 1991. *Analysis of Flow in Water Distribution Networks*.
   Technomic Publishing. Lancaster, PA.

   Clark, R.M. 1998. “Chlorine demand and Trihalomethane formation
   kinetics: a second-order model”, *Jour. Env. Eng*., Vol. 124, No. 1,
   pp. 16-24.

   Dunlop, E.J. 1991. *WADI Users Manual*. Local Government Computer
   Services Board, Dublin, Ireland.

   George, A. & Liu, J. W-H. 1981. *Computer Solution of Large Sparse
   Positive Definite Systems*. Prentice-Hall, Englewood Cliffs, NJ.

   Hamam, Y.M, & Brameller, A. 1971. "Hybrid method for the solution of
   piping networks", *Proc. IEE*, Vol. 113, No. 11, pp. 1607-1612.

   Koechling, M.T. 1998. *Assessment and Modeling of Chlorine Reactions
   with Natural Organic Matter: Impact of Source Water Quality and
   Reaction Conditions*, Ph.D. Thesis, Department of Civil and
   Environmental Engineering, University of Cincinnati, Cincinnati,
   Ohio.

   Liou, C.P. and Kroon, J.R. 1987. “Modeling the propagation of
   waterborne substances in distribution networks”, *J. AWWA*, 79(11),
   54-58.

   Notter, R.H. and Sleicher, C.A. 1971. “The eddy diffusivity in the
   turbulent boundary layer near a wall”, *Chem. Eng. Sci.,* Vol. 26,
   pp. 161-171.

   Osiadacz, A.J. 1987. *Simulation and Analysis of Gas Networks*. E. &
   F.N. Spon, London.

   Rossman, L.A., Boulos, P.F., and Altman, T. (1993). “Discrete
   volume-element method for network water-quality models”, *J. Water
   Resour. Plng. and Mgmt*,, Vol. 119, No. 5, 505-517.

   Rossman, L.A., Clark, R.M., and Grayman, W.M. (1994). “Modeling
   chlorine residuals in drinking-water distribution systems”, *Jour.
   Env. Eng*., Vol. 120, No. 4, 803-820.

   Rossman, L.A. and Boulos, P.F. (1996). “Numerical methods for
   modeling water quality in distribution systems: A comparison”, *J.
   Water Resour. Plng. and Mgmt*, Vol. 122, No. 2, 137-146.

   Rossman, L.A. and Grayman, W.M. 1999. “Scale-model studies of mixing
   in drinking water storage tanks”, *Jour. Env. Eng*., Vol. 125, No. 8,
   pp. 755-761.

   Salgado, R., Todini, E., & O'Connell, P.E. 1988. "Extending the
   gradient method to include pressure regulating valves in pipe
   networks". *Proc. Inter. Symposium on Computer Modeling of Water
   Distribution Systems*, University of Kentucky, May 12-13.

   Todini, E. & Pilati, S. 1987. "A gradient method for the analysis of
   pipe networks". *International Conference on Computer Applications
   for Water Supply and Distribution*, Leicester Polytechnic, UK,
   September 8-10.

.. |image0| image:: .\/media/image1.jpeg
.. |image1| image:: .\/media/image2.png
.. |image2| image:: .\/media/image3.png
.. |image3| image:: .\/media/image4.png
.. |image4| image:: .\/media/image5.png
.. |image5| image:: .\/media/image6.png
.. |image6| image:: .\/media/image7.png
.. |image7| image:: .\/media/image8.png
.. |image8| image:: .\/media/image9.png
.. |image9| image:: .\/media/image10.png
.. |image10| image:: .\/media/image11.png
.. |image11| image:: .\/media/image12.png
.. |image12| image:: .\/media/image13.png
.. |image13| image:: .\/media/image12.png
.. |image14| image:: .\/media/image14.jpeg
.. |image15| image:: .\/media/image15.png
.. |image16| image:: .\/media/image16.jpeg
.. |image17| image:: .\/media/image17.png
.. |image18| image:: .\/media/image18.png
.. |image19| image:: .\/media/image19.png
.. |image20| image:: .\/media/image20.png
.. |image21| image:: .\/media/image21.png
.. |image22| image:: .\/media/image16.jpeg
.. |image23| image:: .\/media/image22.png
.. |image24| image:: .\/media/image18.png
.. |image25| image:: .\/media/image23.png
.. |image26| image:: .\/media/image24.png
.. |image27| image:: .\/media/image25.png
.. |image28| image:: .\/media/image26.png
.. |image29| image:: .\/media/image27.png
.. |image30| image:: .\/media/image28.png
.. |image31| image:: .\/media/image29.png
.. |image32| image:: .\/media/image30.png
.. |image33| image:: .\/media/image31.png
.. |image34| image:: .\/media/image32.png
.. |image35| image:: .\/media/image33.png
.. |image36| image:: .\/media/image34.png
.. |image37| image:: .\/media/image35.png
.. |image38| image:: .\/media/image36.png
.. |image39| image:: .\/media/image37.png
.. |image40| image:: .\/media/image38.png
.. |image41| image:: .\/media/image39.png
.. |image42| image:: .\/media/image40.png
.. |image43| image:: .\/media/image41.png
.. |image44| image:: .\/media/image42.png
.. |image45| image:: .\/media/image43.png
.. |image46| image:: .\/media/image44.png
.. |image47| image:: .\/media/image45.png
.. |image48| image:: .\/media/image18.png
.. |image49| image:: .\/media/image25.png
.. |image50| image:: .\/media/image19.png
.. |image51| image:: .\/media/image46.png
.. |image52| image:: .\/media/image12.png
.. |image53| image:: .\/media/image13.png
.. |image54| image:: .\/media/image47.png
.. |image55| image:: .\/media/image48.png
.. |image56| image:: .\/media/image49.png
.. |image57| image:: .\/media/image50.png
.. |image58| image:: .\/media/image51.png
.. |image59| image:: .\/media/image6.png
.. |image60| image:: .\/media/image5.png
.. |image61| image:: .\/media/image7.png
.. |image62| image:: .\/media/image9.png
.. |image63| image:: .\/media/image10.png
.. |image64| image:: .\/media/image52.png
.. |image65| image:: .\/media/image11.png
.. |image66| image:: .\/media/image53.jpeg
.. |image67| image:: .\/media/image54.jpeg
.. |image68| image:: .\/media/image55.png
.. |image69| image:: .\/media/image57.png
.. |image70| image:: .\/media/image58.png
.. |image71| image:: .\/media/image59.png
.. |image72| image:: .\/media/image38.png
.. |image73| image:: .\/media/image39.png
.. |image74| image:: .\/media/image40.png
.. |image75| image:: .\/media/image2.png
.. |image76| image:: .\/media/image60.png
.. |image77| image:: .\/media/image61.png
.. |image78| image:: .\/media/image6.png
.. |image79| image:: .\/media/image5.png
.. |image80| image:: .\/media/image7.png
.. |image81| image:: .\/media/image16.jpeg
.. |image82| image:: .\/media/image9.png
.. |image83| image:: .\/media/image10.png
.. |image84| image:: .\/media/image52.png
.. |image85| image:: .\/media/image11.png
.. |image86| image:: .\/media/image12.png
.. |image87| image:: .\/media/image14.jpeg
.. |image88| image:: .\/media/image14.jpeg
.. |image89| image:: .\/media/image17.png
.. |image90| image:: .\/media/image22.png
.. |image91| image:: .\/media/image62.png
.. |image92| image:: .\/media/image63.png
.. |image93| image:: .\/media/image64.png
.. |image94| image:: .\/media/image13.png
.. |image95| image:: .\/media/image65.png
.. |image96| image:: .\/media/image44.png
.. |image97| image:: .\/media/image47.png
.. |image98| image:: .\/media/image44.png
.. |image99| image:: .\/media/image66.png
.. |image100| image:: .\/media/image67.png
.. |image101| image:: .\/media/image68.png
.. |image102| image:: .\/media/image49.png
.. |image103| image:: .\/media/image50.png
.. |image104| image:: .\/media/image48.png
.. |image105| image:: .\/media/image42.png
.. |image106| image:: .\/media/image42.png
.. |image107| image:: .\/media/image69.jpeg
.. |image108| image:: .\/media/image70.png
.. |image109| image:: .\/media/image71.png
.. |image110| image:: .\/media/image46.png
.. |image111| image:: .\/media/image72.png
.. |image112| image:: .\/media/image14.jpeg
.. |image113| image:: .\/media/image18.png
.. |image114| image:: .\/media/image73.png
.. |image115| image:: .\/media/image74.png
.. |image116| image:: .\/media/image45.png
.. |image117| image:: .\/media/image25.png
.. |image118| image:: .\/media/image75.png
.. |image119| image:: .\/media/image76.png
.. |image120| image:: .\/media/image77.png
.. |image121| image:: .\/media/image78.png
.. |image122| image:: .\/media/image79.png
.. |image123| image:: .\/media/image80.png
.. |image124| image:: .\/media/image46.png
.. |image125| image:: .\/media/image81.png
.. |image126| image:: .\/media/image82.png
.. |image127| image:: .\/media/image19.png
.. |image128| image:: .\/media/image83.png
.. |image129| image:: .\/media/image84.png
.. |image130| image:: .\/media/image85.png
.. |image131| image:: .\/media/image86.png
.. |image132| image:: .\/media/image46.png
.. |image133| image:: .\/media/image87.png
.. |image134| image:: .\/media/image88.png
.. |image135| image:: .\/media/image46.png
.. |image136| image:: .\/media/image89.png
.. |image137| image:: .\/media/image90.png
.. |image138| image:: .\/media/image46.png
.. |image139| image:: .\/media/image73.png
.. |image140| image:: .\/media/image91.png
.. |image141| image:: .\/media/image41.png
.. |image142| image:: .\/media/image43.png
.. |image143| image:: .\/media/image92.png
.. |image144| image:: .\/media/image93.png
.. |image145| image:: .\/media/image94.png
.. |image146| image:: .\/media/image95.png
.. |image147| image:: .\/media/image96.png
.. |image148| image:: .\/media/image98.png

