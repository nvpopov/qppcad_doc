Project Displacements to Geometry
==================================

Items in workspace:
#######################

ws[0][0] 
    elementary cell

ws[0][1] 
    initial geometry 

ws[0][2] 
    final geometry 

ws[0][3] 
    spherical slab      

Naive approach
#######################


1. Update charges by type:

.. code-block:: python

    cad.gvt.set_charge_for_type(cs(), {"Ba" : 2, "Br" : -1 , "I" : -1})

2. Generate slab from elementary cell:

.. code-block:: python

    cad.gvt.generate_ncells(ws[0][0], -3, 3, -2, 2, -2, 2)

3. Cut spherical segment with center at (0,0,0) with radius = 13 from slab:

.. code-block:: python

    sel.sph(pq.vector3f(0), 13)
    cad.gvt.cut_selected_as_new_gv(cs())

4. Center start and end structures on defect:

.. code-block:: python

    c1 = gp()
    cad.gvt.center_cell_on(cw()[1], c1)
    cad.gvt.center_cell_on(cw()[3], c1)

5. Naive fit two structures by 4 centers:

.. code-block:: python

    cad.gvt.naive_fit_str(cw()[1], cw()[3], [171, 167, 21], [146, 148, 202])

This subroutine will not modify the structure, instead it translates ws_item.

6. Generate compliance list for model and target structure:

.. code-block:: python

    cl1 = cad.gvt.gen_geoms_compliance_list(cw()[1], cw()[3], 0.2)

7. Apply displacements via compliance list for model. Requred initial and final structures:

.. code-block:: python

    cad.gvt.displ_geom_by_compliance_list(cw()[3], cw()[1], cw()[2], cl1)

Where `cw()[0]` - destination, `cw()[1]` - inital geometry, `cw()[2]` - final geometry.

8. Cut QM cluster with center at (0,0,0) with radius = 6 from spherical cluster:

.. code-block:: python

    sel.sph(pq.vector3f(0), 6)
    cad.gvt.cut_selected_as_new_gv(cs())

9. Generate Orca input section - QM atoms:

.. code-block:: python

    tools.to_clipboard(cc.orca.gen_coord_section(cw()[4]))

10. Generate Orca input section - Interface region(Cations - ECP, Anions - PC):

.. code-block:: python

     tools.to_clipboard(cc.orca.gen_coord_section(cw()[0], ["Br", "I"], ["Ba"], {"Ba":"SDD"} ))


