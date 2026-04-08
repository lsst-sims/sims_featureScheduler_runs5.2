Try extending the hour angle mask so we can clean up the last islands of non-observed template area in Y1.

Starting with the templates_fast config

everything in here should be mixed filter pairs.

So, 

* trying a template tier that takes matched blue filter pairs and other filters mixed in pairs
* Setting the template tier to take 20s or 25 s exposures
* have a declination dependent seeing limit
* Have multiple hour angle + area requirements so we clean up isolated islands.
