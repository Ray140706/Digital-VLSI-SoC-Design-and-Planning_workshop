# Digital-VLSI-SoC-Design-and-Planning
## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
## Implementation
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

``
cd Desktop/work/tools/openlane_working_dir/openlane
``

``
docker
``

``
./flow.tcl -interactive
``

``
package require openlane 0.9
``

``
prep -design picorv32a
``

``
run_synthesis
``

Screenshots:

<center>
    <img src="images/section1.png">
</center>

<center>
    <img src="images/prep.png">
</center>

<center>
    <img src="images/synthesis.png">
</center>

2. Calculate the flop ratio.

<center>
    <img src="images/synth_report.png">
</center>

Flop Ratio = Number of D Flip Flops/Total number of cells

Percentage of DFF's = Flop Ratio * 100

Flop Ratio = 1613/14876 = 0.108429685

Percentage of DFF's = 0.108429685 * 100 = 10.8429685%

## Section 2 - Good floorplan vs bad floorplan and introduction to library cells

1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

``
cd Desktop/work/tools/openlane_working_dir/openlane
``

``
docker
``

``
./flow.tcl -interactive
``

``
package require openlane 0.9
``

``
prep -design picorv32a
``

``
run_synthesis
``

``
run_floorplan
``



<center>
    <img src="images/floorplan_run.png">
</center>


<center>
    <img src="images/floorplan.png">
</center>

2. Calculate the die area in microns from the values in floorplan def.

<center>
    <img src="images/floorplan_file.png">
</center>

According to floorplan def

Die width in unit distance = 660685

Die height in unit distance = 671405

Die width in microns = 660685/1000 = 660.685 Microns

Die height in microns = 671405/1000 = 671.405 Microns

Area of die in microns = 660.685 * 671.405 = 443587.212 Square Microns

3. Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal

``
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/
``

``
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
``
