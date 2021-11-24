# Estimating Cosmic Transients from Cosmological Simulations and BPASS

Contains the data products and code to reproduce the figures in [Estimating cosmic Transients from Cosmological Simulations and BPASS](https://arxiv.org/abs/2111.08124)


The data products are contained in a single HDF5 file, excpet the observations which are also provided as `csv` files.

The following data are available in the HDF5 file with the key within the file in brackets

### Delay Time Distributions [DTD]
This group contains the following keys related to the event types.
- BBH
- BHNS
- BNS
- Type Ia
- Type II
- Type IIP
- Type Ib
- Type Ic
- LGRB
- tidal_LGRB
- PISN

Each event type contains 13 datasets with identifiers, which in turn contain an array of length 52 (the number of BPASS timebins)
`['zem5', 'zem4', 'z001', 'z002', 'z003', 'z004', 'z006', 'z008', 'z010', 'z014', 'z020', 'z030', 'z040']`

The DTD contains the attribute `bin_centres` which contains the BPASS bin centres in logspace (log(yrs)). This can be called by doing `f['DTD'].attrs['bin_centres']`. 
For example, bin 6.1 runs from 6.05 to 6.15. However, bin 6.0 runs from ![equation](https://latex.codecogs.com/gif.latex?t%3D0) to ![equation](https://latex.codecogs.com/gif.latex?t%3D10%5E%7B6.05%7D)!


### Star Formation Histories [SFH]
`f['SFH']` contains the star formation histories for the four prescriptions used. Each contains a dataset of a different size depending on the number of snapshots (redshift points) in the simulations. The star formation rates have been normlised to the cosmology used in the paper (![cosmology equation](https://latex.codecogs.com/gif.latex?h%20%3D%200.6766%2C%20%5COmega_M%20%3D%200.3111%2C%20%5COmega_%5CLambda%20%3D0.6889)).

- empirical        (13x500)
- millimillennium  (13x64)
- TNG              (13x100)
- EAGLE            (13x29)

Additional SFH parameters are: 
- `h`:              Hubble parameter
- `omega_M`:        cosmological mass density
- `omega_L`:        effective mass density of dark energy 
And are accessible through `f['SFH'].attrs['h']`

Per SFH the redshift locations are stored in the attributes of the dataset: `f['SFH/TNG'].attrs['redshift']`


### Raw SFH data

This is a more finely binned star formation rate over metallicity and redshift. 


- Binned in 100 metallicity bins 


Predicted event rates

Observations
