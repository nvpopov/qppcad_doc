CP2K
==================================

Input - Coordinates Section
**********************************

Output
**********************************

.. code-block:: javascript

    &MOTION
        &CELL_OPT
            OPTIMIZER CG
            &CG
                &LINE_SEARCH
                    TYPE 2PNT
                &END LINE_SEARCH
            &END CG
        &END CELL_OPT
        &GEO_OPT
            TYPE MINIMIZATION
            MAX_DR    1.0E-04
            MAX_FORCE 1.0E-04
            RMS_DR    1.0E-04
            RMS_FORCE 1.0E-04
            MAX_ITER 200
            OPTIMIZER LBFGS
        &END GEO_OPT
        &PRINT
            &TRAJECTORY LOW
                FILENAME __STD_OUT__
                FORMAT XMOL
            &END TRAJECTORY
            &FORCES LOW
                FILENAME __STD_OUT__
                FORMAT XMOL
            &END FORCES
            &CELL LOW
                FILENAME __STD_OUT__
            &END CELL
        &END PRINT
    &END MOTION