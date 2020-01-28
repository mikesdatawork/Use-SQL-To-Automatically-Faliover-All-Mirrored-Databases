![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Use SQL To Automatically Faliover All Mirrored Databases
**Post Date: October 28, 2015**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>The following SQL logic will automatically failover ALL your mirrored databases. It will first set all your databases to FULL SAFETY, then fail them over. One thing to keep in mind is that after you have run the code you should go to the secondary server and turn off full safety with ALTER DATABASE [MY_DATABASE] SET SAFETY OFF. This will bring them back to High Performance Mode.</p>      



## SQL-Logic
```SQL
use master;
set nocount on
 
declare
@set_full_safety_on_mirror_databases    varchar(max) = ''
select
@set_full_safety_on_mirror_databases    = @set_full_safety_on_mirror_databases +
'alter database [' + cast(DB_NAME(database_id) as varchar(255)) + '] set safety full;' + char(10) from
sys.database_mirroring
where
mirroring_guid is not null
and mirroring_role_desc = 'PRINCIPAL'
 
--exec (@set_full_safety_on_mirror_databases)
select  (@set_full_safety_on_mirror_databases) for xml path(''), type
 
use master;
set nocount on
 
declare
@failover_mirror_databases  varchar(max) = ''
select
@failover_mirror_databases  = @failover_mirror_databases +
'alter database [' + cast(DB_NAME(database_id) as varchar(255)) + '] set partner failover;' + char(10) + char(10) from
sys.database_mirroring
where
mirroring_guid is not null
and mirroring_role_desc = 'PRINCIPAL'
 
--exec (@failover_mirror_databases)
select  (@failover_mirror_databases) for xml path(''), type

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)


   
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

