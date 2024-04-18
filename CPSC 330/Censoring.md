- when the true outcome of some data points is not fully observed or unknown
- common in [[Survival Analysis]]
	- i.e. some data points have not experienced the "event" within the timeframe of the dataset
## Types of censoring
### Right Censoring
- event of interest has not occurred by the end of observation study
- sub-types
	- did everyone join at the same time?
	- are there other reasons data might be censored at random times?
		- e.g. person died
### Left Censoring
- event of interest occurred before the start of observation
- e.g. study on time until machine failure, but machine was already broken when monitoring began
### Interval censoring
- event of interest known to lie within certain interval, but exact time is unknown

[From Wikipedia](https://en.wikipedia.org/wiki/Censoring_(statistics)):
> - Left censoring – a data point is below a certain value but it is unknown by how much.
> - Interval censoring – a data point is somewhere on an interval between two values.
> - Right censoring – a data point is above a certain value but it is unknown by how much.