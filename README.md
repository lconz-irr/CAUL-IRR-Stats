# CAUL-IRR-Stats
Tool to collect numbers for CAUL IRR statistics from DSpace repositories.

This code was developed for the Library Consortium of New Zealand's IRRs.

Note, this code is unofficial; the definition of the metrics in this code may differ from the CAUL definition.

## Metrics

> 1. Number of complete works (1) held in the institutional repository/ies, available externally (open access)
> 2. Number of complete works (2) held in the institutional repository/ies for internal access only (authorized user access or dark archive)
> 3. Total number of complete works (3) held in the IR (1 and 2)
> 4. Number of items (4) held in the institutional repository/ies (metadata records only)
> 5. Total number of metadata records added during the year
> 6. Total number of complete works added during the year
> 7. Total number of items held in the institutional repository/ies (3 and 4)
> 8. Number of accesses to complete works in the institutional repository during the year
> 9. Number of accesses to metadata record items in the institutional repository during the year
> 10. Total number of accesses to items held in the institutional repository (8 and 9).



## How this works / limitations

This code retrieves all "number of" statistics from the DSpace database. It queries the Solr-based statistics to get usage data for each item identified as relevant. DSpace policies for items, bundles and bitstreams are consulted to determine whether a file is a "complete work" or a "metadata record". The repositories for which this code was written don't hold any "complete works for internal access only", so the code does not attempt to detect those. See the comments in IRRStatsController for a starting point to adopt this for other repositories.

## Instructions

Compile the files using maven -- in the CAUL-IRR-Stats directory, run:
    mvn package

This generates a jar file in the `target` subdirectory.

Make the generated jar file available in the DSpace class-path, eg by placing it into `[dspace]/lib`. 

Then run IRRStatsCreator from the command line via the dsrun command: 

    [dspace]/bin/dspace dsrun nz.ac.lconz.irr.dspace.app.irrstats.IRRStatsCreator fromDate toDate filename

where `filename` is the name of the output file. This should probably be preceded by cleaning up bot data from the Solr statistics (`[dspace]/dspace stats-util -u && [dspace]/dspace stats-util -i`).

Dates need to be given as yyyy-MM-dd; start date is inclusive but end date is exclusive (eg for all of 2012, specify 2012-01-01 2013-01-01).

The output will be in CSV format as follows:

    CountPublicFulltext,n
    CountInternalFulltext,n
    CountAnyFulltext,n
    CountMetadataOnly,n
    AddedMetadataOnly,n
    AddedAnyFulltext,n
    CountAll,n
    AccessAnyFulltext,n
    AccessMetadataOnly,n
    AccessAll,n
    
the order of the metrics corresponds to that in the list above, which in turn is quoted from the IRR stats PDF document distributed by CAUL.
