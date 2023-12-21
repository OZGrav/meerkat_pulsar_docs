# OzSTAR Storage

The following is a description of where data is stored and the directory structure for the [processed fold mode data](/meerkat_pulsar_docs/ozstar_storage/#processed-fold-mode-data), [raw fold mode data](/meerkat_pulsar_docs/ozstar_storage/#raw-fold-mode-data) and the [raw search mode data](/meerkat_pulsar_docs/ozstar_storage/#raw-search-mode-data).

The following are used to describe the directory structure:

 - `<pulsar>`: A pulsar J name (e.g. J0437-4715)
 - `<utc>`: The start time of the observation in UTC and the format "YYYY-MM-DD-HH:MM:SS.SS" (e.g. 2023-12-11-03:23:30)
 - `<beam>`: The beam ID (the PTUSE server, e.g. 4)
 - `<freq>`: The centre frequency in MHz (e.g. 1284)


## Processed fold mode data

The processed fold mode data is generated with [MeerPipe](/meerkat_pulsar_docs/meerpipe/) using the raw fold mode data and is stored in the following directory:

```
/fred/oz005/users/nswainst/meerpipe_testing_outputs/<pulsar>/<utc>/<beam>
```

This directory will contain a `results.json` file which are outputs of [MeerPipe](/meerkat_pulsar_docs/meerpipe/) calculations
and either a cleaned/zapped (`<pulsar>_<utc>_zap.ar`) or raw (`<pulsar>_<utc>_raw.ar`) file if no template is available (templates are required for cleaning with [MeerGuard](https://github.com/danielreardon/MeerGuard)).
If there is only a raw file you can either manually clean it with
[paz](https://ozgrav.github.io/2023-09-25_NWU_Pulsar_Timing_Workshop/PulsarData/index.html#pazi)
or [pazi](https://ozgrav.github.io/2023-09-25_NWU_Pulsar_Timing_Workshop/PulsarData/index.html#paz)
or add a template to the [ephemeris and template repo](/meerkat_pulsar_docs/ephem_template/#development-add-or-update-ephemerides-and-templates) so that the [MeerPipe](/meerkat_pulsar_docs/meerpipe/) pipeline can clean it and create the following outputs.

This directory also contains the following subdirectories that will be explained in the following subsections:

 - [`images`](/meerkat_pulsar_docs/ozstar_storage/#images)
 - [`decimated`](/meerkat_pulsar_docs/ozstar_storage/#decimated)
 - [`timing`](/meerkat_pulsar_docs/ozstar_storage/#timing)
 - [`scintillation`](/meerkat_pulsar_docs/ozstar_storage/#scintillation)


### Images

The images directory (`/fred/oz005/users/nswainst/meerpipe_testing_outputs/<pulsar>/<utc>/<beam>/images/`) contains all the images that will be uploaded to the [MeerTime data portal](https://pulsars.org.au/).
Each image will either start with `cleaned` or `raw` depending on whether the image was created from a cleaned/zapped or raw archive respectively.
The images names and descriptions are listed below in the same order they appear on the data portal:

 - `{cleaned/raw}_profile_ftp.png`: The polarisation pulse profile.
 - `{cleaned/raw}_profile_fts.png`: The pulse profile.
 - `{cleaned/raw}_phase_time.png`: The phase vs. time plot.
 - `{cleaned/raw}_phase_freq.png`: The phase vs. frequency plot.
 - `{cleaned/raw}_bandpass.png`: The bandpass plot which shows the frequency response and which channels were flagged.
 - `{cleaned/raw}_SNR_cumulative.png`: The cumulative signal-to-noise ratio plot which shows how the SNR increases with time.
 - `{cleaned/raw}_SNR_single.png`: The single subint signal-to-noise ratio plot which shows the SNR at each subint.


### Decimated

The decimated directory (`/fred/oz005/users/nswainst/meerpipe_testing_outputs/<pulsar>/<utc>/<beam>/decimated/`) contains the decimated archives that are used for the timing analysis for all projects.
The decimated archives have the following naming convention:

```
<pulsar>_<utc>_zap<_chopped>.<nchan>ch<npol>p<nsub>t.ar
```

where `<_chopped>` is included in the file name if the edge frequency channels are removed,
`<nchan>` is the number of frequency channels,
`<npol>` is the number of polarisations and
`<nsub>` is the number of time subintegrations.

For example `J0855-3331_2020-07-11-11:08:23_zap_chopped.16ch1p1t.ar` is a decimated archive with the edge channels removed, 16 frequency channels, 1 polarisation and 1 subintegration.


### Timing

The timing directory contains subdirectories for each project (e.g PTA) (`/fred/oz005/users/nswainst/meerpipe_testing_outputs/<pulsar>/<utc>/<beam>/decimated/<project>/`).
In each of these project timing subdirectories there is a ephemeris file (`<pulsar>.par`) and a template file (`<pulsar>.std`) which are used to make a time of arrival (ToA) `.tim` files.
The ToA files have the following format:

```
<pulsar>_<utc>_zap<_chopped>.<nchan>ch<npol>p<nsub>t.tim
```

where, as above, `<_chopped>` is included in the file name if the edge frequency channels are removed,
`<nchan>` is the number of frequency channels,
`<npol>` is the number of polarisations and
`<nsub>` is the number of time subintegrations.

The ToAs can manually be combined into a single `.tim` file or more easily downloaded using `psrdb toa download`, see the [psrd docs](https://psrdb.readthedocs.io/en/latest/how_to_use.html#toa-download-example) for examples of how to do so.


### Scintillation

The scintillation directory (`/fred/oz005/users/nswainst/meerpipe_testing_outputs/<pulsar>/<utc>/<beam>/scintillation/`) contains files used for scintillation analysis.
The `.dynspec` data files are generated with the `psrflux` psrchive script and have the following naming convention:

```
<pulsar>_<utc>_{raw,zap}.ar.dynspec
```

Where `raw` is for the raw archives and `zap` is for the cleaned/zapped archives.

There are also png files which are created with the [scintools](https://github.com/danielreardon/scintools) repository and have the following naming convention:

```
<pulsar>_<utc>_{raw,zap}.ar.dynspec.png
```


## Raw fold mode data

The raw fold mode data is stored in the following directory:

```
/fred/oz005/timing/<pulsar>/<utc>/<beam>/<freq>
```

Which contains:

 - `*.ar`: The raw fold mode archives in ~8 second sub-integrations.
 - `obs.header`: The metadata file output by the PTUSE.
 - `meertime.json`: The metadata file in the format required by the MeerTime data portal which was created by `psrdb`.
 - `obs.finished`: Empty file that is used to indicate the observation has finished transferring to OzSTAR.

## Raw search mode data

The raw search mode data is stored in the following directory:

```
/fred/oz005/search/<pulsar>/<utc>/<beam>/<freq>
```

Which contains:

 - `*.sf`: The raw search mode archives in ~8 second sub-integrations.
 - `obs.header`: The metadata file output by the PTUSE.
 - `meertime.json`: The metadata file in the format required by the MeerTime data portal which was created by `psrdb`.
 - `obs.finished`: Empty file that is used to indicate the observation has finished transferring to OzSTAR.
 - `transfer.list`: List of files that have been transferred to OzSTAR.
