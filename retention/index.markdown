---
layout: default	
title: retention
subtitle: An R package for estimating and visualizing retention and completion rates for postsecondary institutions.
submenu: retention
---

### Overview

More documentation coming soon...

<a href='CohortRetention.pdf'><img src='CohortRetention.png' width='600' alt='Cohort Retention' border='0'></a>
<a href='Retention.pdf'><img src='Retention.png' width='600' alt='Overall Retention' border='0'></a>

	data(students)
	data(graduates)

	names(students)
	names(graduates)
	str(students)
	str(graduates)

	#Recode the persisting column to be of logical type
	students$Persisting = NA
	students[which(students$Persist == 'Y'),'Persisting'] = TRUE
	students[which(students$Persist == 'N'),'Persisting'] = FALSE
	
### Paper presented at NEAIR

Bryer, J.M., & Daniels, L. (2011). [Measuring All Students: An Alternative Method for Retention and Completion Rates](http://www.neair.org/resource/resmgr/confpapers2011/tues1_paper_camBryerDaniels.pdf). Paper presented at the North East Association for Institutional Research 28th Annual Conference. Boston, MA.
