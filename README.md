# Estimating Cosmic Transients from Cosmological Simulations and BPASS

Contains the data products and code to reproduce the figures in [Estimating cosmic Transients from Cosmological Simulations and BPASS](https://arxiv.org/abs/2111.08124)


## Processing
An example on how the event rates are processed can be found [here](https://github.com/UoA-Stars-And-Supernovae/transient_rates_from_cosmological_simulations/blob/main/processing/event_rate_calculation_example.ipynb). It goes through the calculation of the electromagnetic and gravitational wave transients for the TNG simulation. Other prescriptions can be used in a similar manner. 

## Data
The data products are contained in a single [HDF5 file](https://github.com/UoA-Stars-And-Supernovae/transient_rates_from_cosmological_simulations/blob/main/data/data.h5), except the observations which are also provided as `csv` [files](https://github.com/UoA-Stars-And-Supernovae/transient_rates_from_cosmological_simulations/tree/main/data/observations).

The following data are available in the HDF5 file with the key within the file in brackets

### Delay Time Distributions [DTD]
This group contains the following keys related to the event types.
- `BBH`
  - `zem5`
  - `zem4`
  - `z001`
  - `z002`
  - `z003`
  - etc (see below) 
- `BHNS`
- `BNS`
- `Type Ia`
- `Type II`
- `Type IIP`
- `Type Ib`
- `Type Ic`
- `LGRB`
- `tidal_LGRB`
- `PISN`

Each event type contains 13 datasets with identifiers, which in turn contain an array of length 52 (the number of BPASS timebins)
`['zem5', 'zem4', 'z001', 'z002', 'z003', 'z004', 'z006', 'z008', 'z010', 'z014', 'z020', 'z030', 'z040']`

The DTD contains the attribute `bin_centres` which contains the BPASS bin centres in logspace (log(yrs)). This can be called by doing `f['DTD'].attrs['bin_centres']`. 
For example, bin 6.1 runs from 6.05 to 6.15. However, bin 6.0 runs from ![equation](https://latex.codecogs.com/gif.latex?t%3D0) to ![equation](https://latex.codecogs.com/gif.latex?t%3D10%5E%7B6.05%7D)!


### Star Formation Histories [SFH]
`f['SFH']` contains the star formation histories for the four prescriptions used. Each contains a dataset of a different size depending on the number of snapshots (redshift points) in the simulations. The star formation rates have been normlised to the cosmology used in the paper (![cosmology equation](https://latex.codecogs.com/gif.latex?h%20%3D%200.6766%2C%20%5COmega_M%20%3D%200.3111%2C%20%5COmega_%5CLambda%20%3D0.6889)).

- `empirical`        (13x500)
- `millimillennium`  (13x64)
- `TNG`              (13x100)
- `EAGLE`            (13x29)

Additional SFH parameters are: 
- `h`:              Hubble parameter
- `omega_M`:        cosmological mass density
- `omega_L`:        effective mass density of dark energy 

And are accessible through `f['SFH'].attrs['h']`


Per SFH the redshift locations are stored in the attributes of the dataset: `f['SFH/TNG'].attrs['redshift']`


### Raw SFH data [raw_SFH]

This is a more finely binned star formation rate over metallicity and redshift. Instead of 13 BPASS metallicities there are 100 metallicity bins and the mean, median and 16%-84% interval is recorded.

The group contains the following structure with each SFH containing the same datasets. The `distribution` is normalised to the cosmology used in the paper.

- `empirical`
  - `distribution` : The binned SFR distribution over redshift and metallicity (100x500)
  - `mean`: The SFR-weighted mean metallicity over redshift (500)
  - `median`: The SFR-weighted median metallicity over redshift (500)
  - `16p`: 16% of the SFR-weighted metallicity distribution over redshift (500)
  - `84p`: 84% of the SFR-weighted metallicity distribution over redshift (500)
- `millimillennium` (100x64)
- `TNG` (100x100)
- `EAGLE` (100x29)

The following attributes are available per SFH:
- `metallicity_edges`
- `redshift`

Axxessible using `f['raw_SFH/TNG'].attrs['metallicity_edges']` for each SFH.

### Predicted event rates [event_rates]

The predicted event rates are created by combining the cosmic SFH with BPASS DTDs, as described in this notebook.
This group contains the predicted rates for each event type per BPASS metallicity. Each simulation contains predicted rates for all event types mentioned in the DTD section on this page. 

- `empirical`
  - `BBH` (13x100) 
  - `BHNS` (13x100)
  - etc
- `millimennium`  (13x100)
- `TNG`           (13x100)
- `EAGLE`         (13x100)

The following attributes can be accessed per SFH:
- `event_lookbacktime`: The lookback time bin edges of the calculated rates
- `event_redshift`:     The redshift bin edges of the calculated rates
- `h`:                  Hubble parameter
- `omega_M`:            cosmological mass density 
- `omega_L`:            effective mass density of dark energy

While `event_redshift` and `event_lookbacktime` are accessible per SFH, they are the same for each SFH.


### Observations [observations]

The raw observations can be found ![here](https://github.com/UoA-Stars-And-Supernovae/transient_rates_from_cosmological_simulations/tree/main/data/observations), but the are also available within the HDF5 file.
It's important to note that the cosmology has not been applied yet! 

The structure of this section in the HDF5 file is: 

- `Ia`
  - `mean_z`
  - `lower_z`
  - `upper_z`
  - `rate`
  - `lower_rate`
  - `upper_rate`
- `CCSN`
- `SESN`
- `II`
- `LGRB` (normalised using the geometric parameters and lighcurve correction as mentioned in the paper)
- `PISN`
- `BBH`
- `BHNS`
- `BNS`

The compact object groups also contain a `data_description` attribute, which describes which models are at which index, and can be accessed like `f['observations/BBH'].attrs['data_description']`.
