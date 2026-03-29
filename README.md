# Digital-VLSI-SoC-Design-and-Planning
## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis:

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

### 2. Calculate the flop ratio.

<center>
    <img src="images/synth_report.png">
</center>

Flop Ratio = Number of D Flip Flops/Total number of cells

Percentage of DFF's = Flop Ratio * 100

Flop Ratio = 1613/14876 = 0.108429685

Percentage of DFF's = 0.108429685 * 100 = 10.8429685%

## Section 2 - Good floorplan vs bad floorplan and introduction to library cells

### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

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

### 2. Calculate the die area in microns from the values in floorplan def.

<center>
    <img src="images/floorplan_file.png">
</center>

According to floorplan def

Die width in unit distance = 660685

Die height in unit distance = 671405

Die width in microns = 660685/1000 = 660.685 Microns

Die height in microns = 671405/1000 = 671.405 Microns

Area of die in microns = 660.685 * 671.405 = 443587.212 Square Microns

### 3. Load generated floorplan def in magic tool and explore the floorplan.

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

### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command run placement:

``
run_placement
``

<center>
    <img src="images/Placement_run.png">
</center>

### 5. Load generated placement def in magic tool and explore the placement.

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

### 1. Clone custom inverter standard cell design from github repository

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

### 2. Load the custom inverter layout in magic and explore.

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

### 3. Spice extraction of inverter in magic.

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

### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

<center>
    <img src="images/Layout_unitDistance.png">
</center>

Final edited spice file ready for ngspice simulation

<center>
    <img src="images/SpiceFile_edited.png">
</center>

### 5. Post-layout ngspice simulations

Commands for ngspice simulation

``
ngspice sky130_inv.spice
``

``
plot y vs time a
``

 ngspice run

 <center>
    <img src="images/ngspice run.png">
</center>

<center>
    <img src="images/ngspice run(1).png">
</center>

Generated plot

<center>
    <img src="images/ngspice_plot.png">
</center>

Rise transition time calculation

Rise transition time = Time take for output to rise 80% - Time taken for output to rise 20%

20% of output = 660mV

80% of output = 2.64V

20% Screenshots

<center>
    <img src="images/20.png">
</center>

<center>
    <img src="images/20_op.png">
</center>

80% Screenshots

<center>
    <img src="images/80.png">
</center>

<center>
    <img src="images/80_op.png">
</center>

Rise transition time = 2.24353 - 2.18242 = 0.06111 ns = 61.11 ps

Fall transition time = 4.0955 - 4.0536 = 0.0419 ns = 41.9 ps

Rise cell delay = Time taken for output to rise to 50% - Time taken for input to fall to 50%

50% 0f 3.3V = 1.65V

Fall Cell Delay = 4.07 - 4.05 = 0.02 ns = 20ps

50% Screenshots

<center>
    <img src="images/50.png">
</center>

<center>
    <img src="images/50_op.png">
</center>

### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

``
cd
``

``
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
``

``
tar xfz drc_tests.tgz
``

``
cd drc_tests
``

``
ls -al
``

``
gvim .magicrc
``

``
magic -d XR &
``

<center>
    <img src="images/drc_cmds.png">
</center>

.magicrc file

<center>
    <img src="images/magicrc_file.png">
</center>

Incorrectly implemented poly.9 simple rule correction

 No drc violation even though spacing < 0.48u

 <center>
    <img src="images/poly.9.png">
</center>

New commands inserted in sky130A.tech file to update drc

 <center>
    <img src="images/drc_update(1).png">
</center>

 <center>
    <img src="images/drc_update(2).png">
</center>

Commands to run in tkcon window

``
tech load sky130A.tech
``

``
drc check
``

``
drc why
``

 <center>
    <img src="images/poly_update.png">
</center>

Incorrectly implemented difftap.2 simple rule correction

 No drc violation even though spacing < 0.42u

 <center>
    <img src="images/difftap.2.png">
</center>

New commands inserted in sky130A.tech file to update drc

<center>
    <img src="images/difftap.2_drc(update).png">
</center>

<center>
    <img src="images/difftap(updated).png">
</center>

Incorrectly implemented nwell.4 complex rule correction

 No drc violation even though no tap present in nwell

 <center>
    <img src="images/nwell_incorrect.png">
</center>

New commands inserted in sky130A.tech file to update drc

 <center>
    <img src="images/nwell.4_drcUpdate.png">
</center>

 <center>
    <img src="images/nwell.4_drcUpdate(2).png">
</center>

``
Commands to run in tkcon window
``

``
tech load sky130A.tech
``

``
drc style drc(full)
``

``
drc check
``

``
drc why
``

<center>
    <img src="images/nwell(updated).png">
</center>

## Section 4 - Pre-layout timing analysis and importance of good clock tree

### 1. Fix up small DRC errors and verify the design is ready to be inserted into the flow.

Conditions to be verified before moving forward with custom designed cell layout:

Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.

Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.

Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.

Commands to open the custom inverter layout

``
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
``

``
magic -T sky130A.tech sky130_inv.mag &
``

tracks.info of sky130_fd_sc_hd

<center>
    <img src="images/tracks.info(sky130_fd_sc_hd).png">
</center>

Commands for tkcon window to set grid 

``
help grid
``

``
grid 0.46um 0.34um 0.23um 0.17um
``

<center>
    <img src="images/gridSetting.png">
</center>

Condition 1 verified

<center>
    <img src="images/Inverter_condn1.png">
</center>

Condition 2 verified

<center>
    <img src="images/Inverter_condn2.png">
</center>

Horizontal track pitch = 0.46 um

Width of standard cell = 1.38 um = 0.46 * 3

Condition 3 verified

<center>
    <img src="images/Inverter_condn3.png">
</center>

Vertical track pitch = 0.34 um 

Height of standard cell = 2.72 um = 0.34 * 8

### 2. Save the finalized layout with custom name and open it.

Command for tkcon window to save the layout with custom name

``
save sky130_vsdinv.mag
``

Command to open the newly saved layout

``
magic -T sky130A.tech sky130_vsdinv.mag &
``

Newly saved layout

<center>
    <img src="images/Inverter_new.png">
</center>

### 3. Generate lef from the layout

Command for tkcon window to write lef

``
lef write
``

<center>
    <img src="images/lef_write.png">
</center>

<center>
    <img src="images/lef_new.png">
</center>

### 4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

``
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
``

``
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
``

``
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
``

``
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
``

<center>
    <img src="images/lef_copy.png">
</center>

### 5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow

``
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
``

Edited config.tcl to include the added lef and change library to ones we added in src directory


<center>
    <img src="images/config_edited.png">
</center>

### 6. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis

``
cd Desktop/work/tools/openlane_working_dir/openlane
docker
``

``
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
``


<center>
    <img src="images/inverter_prep.png">
</center>

<center>
    <img src="images/synth_inverter.png">
</center>

### 7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Current design values generated before modifying parameters to improve timing

<center>
    <img src="images/synth_inverter(init).png">
</center>

<center>
    <img src="images/synth_inverter.png">
</center>

Commands to view and change parameters to improve timing and run synthesis

``
prep -design picorv32a -tag 25-03_16-27 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING)
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
``

 Area has increased and worst negative slack has become 0


<center>
    <img src="images/inv_newArea.png">
</center>


<center>
    <img src="images/inv_new.png">
</center>

### 8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Commands to run the floorplan

``
init_floorplan
place_io
tap_decap_or
``

Command to run placement

``
run placement
``

Commands to load placement def in magic

``
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_16-27/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
``

Placement def in magic


<center>
    <img src="images/def(inv).png">
</center>


<center>
    <img src="images/inv_placement.png">
</center>

Command for tkcon window to view internal layers of cells

``
expand
``

Abutment of power pins with other cell from library

<center>
    <img src="images/inv_powerpins.png">
</center>

### 9. Do Post-Synthesis timing analysis with OpenSTA tool.

Commands to add new lef to OpenLANE flow

``
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis
``

Commands to run STA 

``
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
``

<center>
    <img src="images/newlef_STA(1).png">
</center>

<center>
    <img src="images/newlef_STA(2).png">
</center>

<center>
    <img src="images/newlef_STA(3).png">
</center>

<center>
    <img src="images/newlef_STA(4).png">
</center>

### 10. Make timing ECO fixes to remove all violations.


