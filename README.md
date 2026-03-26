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
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/21-03_01-37/results/floorplan/
``

``
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
``

Floorplan def in magic

<center>
    <img src="images/floorplan_def.png">
</center>

Equidistant placement of ports

<center>
    <img src="images/Equidistant pins.png">
</center>

Port layer as set through config.tcl

<center>
    <img src="images/port layer.png">
</center>

<center>
    <img src="images/port layer(2).png">
</center>

Decap Cells and Tap Cells

<center>
    <img src="images/Decap and Tap cells.png">
</center>

Diagonally equidistant Tap cells

<center>
    <img src="images/Diagonal_tapcells.png">
</center>

Unplaced standard cells at the origin

<center>
    <img src="images/unplaced standardcells.png">
</center>

4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command run placement:

``
run_placement
``

<center>
    <img src="images/Placement_run.png">
</center>

5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

``
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/22-03_10-56/results/placement/
``

``
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
``

Floorplan def in magic

<center>
    <img src="images/placement_def.png">
</center>

Standard cells legally placed

<center>
    <img src="images/StandardCell_placement.png">
</center>

## Section 3 - Design library cell using Magic Layout and ngspice characterization

1. Clone custom inverter standard cell design from github repository

``
cd Desktop/work/tools/openlane_working_dir/openlane
``

``
git clone https://github.com/nickson-jose/vsdstdcelldesign
``

``
cd vsdstdcelldesign
``

``
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
``

``
ls
``

``
magic -T sky130A.tech sky130_inv.mag &
``

<center>
    <img src="images/GitRepo_clone.png">
</center>

2. Load the custom inverter layout in magic and explore.

Custom inverter layout in magic

<center>
    <img src="images/CustomInverter_Layout.png">
</center>

NMOS and PMOS identified

<center>
    <img src="images/nmos.png">
</center>

<center>
    <img src="images/pmos.png">
</center>

Output Y connectivity to PMOS and NMOS drain verified

<center>
    <img src="images/DrainConnectivity_Y.png">
</center>

PMOS source connectivity to VDD (here VPWR) verified

<center>
    <img src="images/Vdd-source(pmos).png">
</center>

NMOS source connectivity to VSS (here VGND) verified

<center>
    <img src="images/Vss-source(nmos).png">
</center>

3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

``
pwd
``

``
extract all
``

``
ext2spice cthresh 0 rthresh 0
``

``
ext2spice
``

tkcon window after running above commands

<center>
    <img src="images/spiceExtraction_inverter.png">
</center>

Created spice file

<center>
    <img src="images/SpiceFile.png">
</center>

4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

<center>
    <img src="images/Layout_unitDistance.png">
</center>

Final edited spice file ready for ngspice simulation

<center>
    <img src="images/SpiceFile_edited.png">
</center>

