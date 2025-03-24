# HW3
## Question 2

|                                     | Primary (built on PK)                                                                                                              | Secondary (built on SK)                                                                                                           | Clustered (records are physically adjacent and sorted/partitioned)                                                             | Unclustered (records are scattered on disk)                                                                                                                                                |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Dense (DE per record)               | Possible. The attribute on which the index is built doesn't impose any requirements on whether the index entries are dense or not. | Possible. The attribute on which the index is built doesn't impose any requirement on whether the index entries are dense or not. | Fine. Though possibly inefficient since we don't need DE for every record since there is some order to their physical location | Necessary (to be dense if unclustered). We cannot exploit physical proximity to reduce number of DEs. i.e. we must have a DE per record since they are all stored in random disk locations |
| Sparse (DE per page of records)     | "".                                                                                                                                | ""                                                                                                                                | Good. Can exploit physical ordering to reduce number of DEs required to reach all records                                      | Not possible. No guarantee that records are sorted/partitioned physically, so we need a handle on every record.                                                                            |
| Alt1 (DEs are physical records)     | Possible. Implies the index is clustered on PK                                                                                     | Possible. Implies the index is clustered on PK                                                                                    | Necessary. Index is useless if its DEs are not sorted/partitioned. Alt1 DEs are inherently clustered                           | Not possible. Index is useless if its DEs are not sorted/partitioned. Alt1 DEs are inherently clustered                                                                                    |
| Alt2 (DEs point to records on disk) | Possible.                                                                                                                          | fine                                                                                                                              | fine                                                                                                                           | fine                                                                                                                                                                                       |

### Dense
### Dense Primary Alt1 Clustered
### Dense Primary Alt1 Unclustered
### Dense Primary Alt2 Clustered
### Dense Primary Alt2 Unclustered
### Dense Secondary Alt1 Clustered
### Dense Secondary Alt1 Unclustered
### Dense Secondary Alt2 Clustered
### Dense Secondary Alt2 Unclustered
### Sparse Primary Alt1 Clustered
### Sparse Primary Alt1 Unclustered
- cannot be sparse and unclustered
### Sparse Primary Alt2 Clustered
### Sparse Primary Alt2 Unclustered
- cannot be sparse and unclustered
### Sparse Secondary Alt1 Clustered
### Sparse Secondary Alt1 Unclustered
- cannot be sparse and unclustered
### Sparse Secondary Alt2 Clustered
### Sparse Secondary Alt2 Unclustered
- cannot be sparse and unclustered