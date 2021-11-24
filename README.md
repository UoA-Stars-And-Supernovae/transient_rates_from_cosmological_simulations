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

The DTD contains the attribute `bin_centres` which contains the BPASS bin centres in logspace (log(yrs)). 
For example, bin 6.1 runs from 6.05 to 6.15. However, bin 6.0 runs from ![equation](https://latex.codecogs.com/gif.latex?t%3D0) to ![equation](https://latex.codecogs.com/gif.latex?t%3D10%5E%7B6.05%7D)!


Star Formation Histories
- In BPASS metallicities


Raw SFH data
- Binned in 100 metallicity bins 


Predicted event rates

Observations
