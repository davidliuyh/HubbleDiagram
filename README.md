# Hubble Diagram

## Dependencies  
Install the required Python packages according to `requirements.txt`.

## File Organization  
1. **data** contains the data used in the project:  
   - `DES-Dovekie_HD.csv` contains the SNe used to plot the Hubble diagram.  
   - `.FITS` files are the complete supernova data.  
   - `STAT+SYS.npz` is the covariance provided by Dovekie.  
   - `survey-bricks.fits` provides the bricks from the Legacy Survey, which can be used to query which brick an object at a given RA/Dec belongs to.  

2. **fits** stores the model fitting results.  

3. **host_galaxy** contains the galaxies within a 20.0 arcsec radius search around each SN (note: these are not necessarily the host galaxies).  

4. **post_data** stores processed valid host galaxies.

5. **tractor_data** is a temporary directory used to download LSC data when searching for the nearest galaxies.  

## hubble.ipynb  
### Read Data  
Organize the complete information of the SNe used from `DES-Dovekie_HD.csv` into the `SN_sample` table, maintaining the order indicated by the `ID`. Note: This order must not be changed because the covariance stored in `STAT+SYS.npz` follows this ordering.  

### Hubble Diagram  
Plot the Hubble diagram.  

### Fit Cosmology  
Use Ultranest to fit different cosmological models. Currently, the Flat w0waCDM model still has some issues.  

### Crossmatch  
#### Match LSC Bricks  
For all SNe in `SN_sample`, find their corresponding LSC bricks and record them in the `'SKY'` and `'BRICKNAME'` columns.  

#### Find Nearest Galaxy for a SN  
Search for the nearest galaxy within a 20.0 arcsec radius. The resulting galaxy information is stored as a table in `host_galaxy`. This table's `'ID'` corresponds to the `'ID'` in `SN_sample`, `'type'` indicates the source (galaxy) type, and `'dist'` is the distance to the corresponding SN. The files are saved with the `.fits` extension and the `ID` as the suffix. This step has already been completed and does not need to be run again.  

#### Find Host Galaxy for a SN  
Galaxies with `dist < 1.0 arcsec` are considered host galaxies. Only 1196 such galaxies were found. The instructor mentioned that it is not necessary to find a host for every SN. The final result is the `valid_host_galaxies` list, where each element is a table of host galaxy information to be used for subsequent processing.  

## residual_w1.ipynb  
Attempts to identify systematic correlations between Hubble residuals and W1 magnitude.  

## residual_morphology.ipynb  
Attempts to identify systematic correlations between Hubble residuals and host galaxy morphological features.  

## residual_type&inclination.ipynb  
Attempts to identify systematic correlations between Hubble residuals and host galaxy type and inclination.  

## residual_mass_step.ipynb  
Reproduces the mass step and examines systematic correlations between Hubble residuals and the mass of host galaxy.