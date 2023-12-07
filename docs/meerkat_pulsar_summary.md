# MeerKAT Pulsar Processing Summary

The following is a high level summary of how we turn radio waves into processed pulsar data displayed on the [MeerTime Data Portal](pulsars.org.au).
This summary will include links to the other parts of this documentation and other resources that go into more detail of each aspect of the process.
Many of these subsections will have one of the following labels to help you find the information you desire quicker:

 - **user**: User documentation is for someone who wants to use the pipeline or webpages so needs to know how to load software, what options are available and where to find data.
 - **science**: Scientist documentation is for someone who wants to understands the methods and math used in the process to increase their understanding or find potential issues
 - **dev**: Developer documentation is for someone who wants to understand how the software works so they can add new features.

The high level steps are the following:

- Observed with MeerKAT
- Preprocessed with the PTUSE
- Transferred to OzSTAR
- Observation metadata uploaded to [MeerTime Data Portal](pulsars.org.au)
- Data processed using MeerPipe
- Observation results uploaded to [MeerTime Data Portal](pulsars.org.au)

![meerkat_high_level](figures/meerkat_high_level.png)