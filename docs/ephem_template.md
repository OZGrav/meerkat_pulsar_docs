# MeerTime Ephemerides and Templates

The ephemerides and templates used by the different MeerTime project (PTA, TPA, RelBin and GC) are all stored in a [private repository](https://github.com/OZGrav/meertime_ephemerides_and_templates).

## Motivation

The goal of this repository is to store all ephemerides and templates in one place to avoid duplication of work and make it clear how templates and ephemerides were created.
Making the repository private prevents the values in the ephemerides being scoped by other researchers.
It also allows control of the quality of ephemerides and templates by using automated testing of the files before they are pulled into the main branch and automated creation scripts to make the process more consistent.
The central place will also make it easier for researchers in charge of rerunning processing pipelines to know when to reprocess pulsars and this may even be automated in the future.

## How to use

Once the python scripts are [installed](https://github.com/OZGrav/meertime_ephemerides_and_templates#installation) you can return the path of the ephemeris for a particular pulsar and project (PTA, TPA, RelBin or GC) using:

```
grab_ephemeris <pulsar> --project <project>
```

This will grab a project specific ephemeris if it is available or the default ephemeris if it is not.

You can use a similar command to grab the template and you can also specify the band (LBAND, UHF, SBAND_0, SBAND_1, SBAND_2, SBAND_3, SBAND_4)

```
grab_template <pulsar> --project <project> --band <band>
```

## Layout

The repository is layed out like so:

```
.
├── ephem_template
│   ├── __init__.py
│   ├── python_grabber.py
│   ├── scripts
│   │   ├── grab_ephemeris.py
│   │   ├── grab_template.py
│   │   └── __init__.py
│   ├── ephemeris
│   │   ├── PTUSE
│   │   │   ├── J0437-4715.par
│   │   │   └── ...
│   │   └── TPA
│   │       ├── J0437-4715.par
│   │       └── ...
│   └── template
│       ├── LBAND
│       │   └── TPA
│       │       ├── J0437-4715.std
│       │       └── ...
│       └── UHF
│           └── TPA
│               ├── J0437-4715.std
│               └── ...
├── dev_scripts
│   └── create_ephemeris.sh
├── README.md
└── setup.py
```

The `ephem_template/ephemeris` directory contains the timing ephemerides split into project subdirectories


The `ephem_template/template` directory contains the standard templates split into observing band subdirectories then project subdirectories

The `PTUSE` project is not for a specific project and often has folding ephemerides taken from the raw archives with the `vap -E` command.

The python scripts in `ephem_template` are simple scripts to decide which ephemeris or template to return based on the input pulsar name, project and observing band.

The bash scripts in `dev_scripts` create or rename ephemerides and templates in a consistent way then put them into the correct directory to make development easier and more consistent.

## Development (add or update ephemerides and templates)

To access the repository you must request to become a member of the GitHub team [Meerkat Pulsar Timing](https://github.com/orgs/OZGrav/teams/meerkat-pulsar-timing) by emailing Matthew Bailes or Nicholas Swainston.

How to update the OzSTAR module is described in the repositories [README.md](https://github.com/OZGrav/meertime_ephemerides_and_templates/blob/main/README.md)