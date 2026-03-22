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


