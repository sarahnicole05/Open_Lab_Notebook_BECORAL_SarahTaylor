# Protein Assay (Bicinchoninic Acid (BCA) Assay, F. Prada, 2 March 2022

*A highly sensitive, detergent-compatible, colorimetric method for quantifying total protein concentration (typically 20–2,000 ug/mL). It works by reducing Cu2+ to Cu1+ in alkaline conditions (biuret reaction), which then chelates with BCA to form a purple-colored complex that absorbs light at 562 nm.*

Use the [Pierce BCA Protein Assay Kit](https://www.thermofisher.com/order/catalog/product/23225). See also the kit [User Guide](https://documents.thermofisher.com/TFS-Assets/LSG/manuals/MAN0011430_Pierce_BCA_Protein_Asy_UG.pdf).

### STANDARD CURVE PREPARATION (BOVINE SERUM ALBUMIN (BSA) STANDARD)

STOCK SOLUTION: 2 mg/mL BSA

1. Transfer stock solution from the glass ampuole to a pre-labeled Epi tube. Unused stock can be stored at -20C.

2. Prelabel 1 ml Epi tubes according to the table below.

3. Add PBS according to the table below first. Then add BSA stock solution.
   
| Source |   BSA  |  PBS  | Final Concentration (ug/ml)| Tube ID |
|---|---|---|---|---|
| Stock | 1000 ul | - | 2000 ug/ml | A |
| A | 500 ul | 500 ul | 1000 ug/ml | B |
| A | 250 ul | 750 ul | 500 ug/ml | C |
| B | 250 ul | 750 ul | 250 ug/ml | D |
| C | 250 ul | 750 ul | 125 ug/ml | E |
| D | 200 ul | 800 ul | 50 ug/ml | F |
| E | 200 ul | 800 ul | 25 ug/ml | G |
| PBS | - | 1000 ul | 0 ug/ml | H |

4. BSA standard curve can be stored at 4C for up to one year.


### SAMPLE LOADING ON MICROPLATES.

6. Use [clear flat-bottom 96 well plates](https://www.fishersci.com/shop/products/fisherbrand-96-well-polystyrene-plates-7/12565501#?keyword=clear%20bottom%2096%20well%20plate).

7. Store all BSA standard tubes and sample tubes on ice while preparing the assay.

8. Load wells A-H in row 1 of the microplate with 25 ul each of BSA standards A-H. Briefly vortex and then centrifuge each standard tube before pipetting.

9. If frozen, gently thaw, then briefly vortex and centrifuge each sample. Pipette duplicate volumes of 25 ul of each sample into the microplate. If you anticipate exceptionally high protein concentrations in your samples, you can dilute them 1:1 (or more) in PBS within the microplate wells.

10. Calculate the total volume of working reagent (WR) needed for all standard plus sample wells. Each well gets 200 ul WR. The final WR is a mix of 50:1 WR A: WR B.

(8 standard wells + n sample wells + 2 "just-in-case" wells) * 200 = ul WR A

(Volume WR A)/50 = ul WR B

11. Mix both WRs by gently inverting them 3 times. In a conical vial or reagent mixing tray, pipette WR A and B volumes and then gently mix.

12. Cover (with parafilm or a sticking foil/film to prevent evaporation) and incubate the microplate at 37C for 30 minutes.

13. Analyze on a spectrophotometer platereader (SoftMax Pro 6.2.1 in the Falko lab on the 3rd floor of DMCS) after removing the cover.


### SOFTWARE ANALYSIS IN THE SOFTMAX PRO SOFTWARE

14. Turn on the microplate reader and allow the lamp to warm up while the samples are incubating for 30 minutes (step 12 above).
   
15. On the top left of the SoftMax Pro window, open NEW DATA FILE.
    
16. On the top of the new window, select the third icon TEMPLATE EDITOR. 

17. Select the first cell (A1), select STANDARDS on the right, add concentration, click on ASSIGN.

19. Repeat this for all the wells with STANDARDS on the first column.

20. On the second column, click on the first cell and select UNKNOWNS (our samples IOM, SOM, or PBS).

21. Settings -> 562 nm wavelength.

22. Settings -> READ AREA -> select the wells we want to measure.

23. OPTIONAL: Set the microplate to shake in the plate reader for 5 minutes.

24. Press Read.

25. Always check the calibration curve. R2 should be >0.99. If needed, remove the final points on the regression through TEMPLATE EDITOR.
  
26. To see the actual concentrations of proteins in our samples, click on unknowns.

27. Either export or record absorbances/calculated concentrations of all standards plus samples.
