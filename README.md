![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 使用TSQL查找所有内容（Content）数据库中的Sharepoint文件
#### Find Sharepoint Files Across All Content Databases With TSQL
**发布-日期: 2018年6月14日 (评论)**

![#](images/find-sharepoint-files-across-all-content-databases-with-sql.png?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
在下面的示例中，是一些能让你在所有Sharepiont内容数据库中搜索文件的基本SQL逻辑（logic）。


## English
In the example below you’ll find some basic SQL logic that allows you to search for documents across all Sharepiont Content Databases.

---
## Logic
```SQL
use master;
set nocount on
 
declare @gather     varchar(max)
set @gather     = ''
select  @gather     = @gather + 
'
use [' + [name] + '];' + char(10) + 
'set nocount on' + char(10) + 
'select 
    ''database''        = db_name()
,   ''time_created''    = alldocs.timecreated
,   ''list_name''       = alllists.tp_title
,   ''file_name''       = alldocs.leafname
,   ''url''         = alldocs.dirname
,   ''kb''          = (convert(bigint,alldocstreams.size))/1024
,   ''mb''          = (convert(bigint,alldocstreams.size))/1024/1024
from 
    alldocs join alldocstreams  on alldocs.id=alldocstreams.id 
    join alllists           on alllists.tp_id = alldocs.listid
where
    right([alldocs].[leafname], 2) in (''oc'', ''cx'', ''df'', ''sg'', ''xt'')
    and alldocs.leafname like ''%design%''
order by
    alldocs.leafname asc
' + char(10) + char(10)
from    sys.databases where [name] like '%content%'
 
if object_id('tempdb..##gathersummary') is not null
    drop table  ##gathersummary
create table        ##gathersummary
(
    [database]  varchar(255)
,   [time_created]  datetime
,   [list_name] varchar(555)
,   [file_name] varchar(555)
,   [url]       varchar(max)
,   [kb]        int
,   [mb]        int
)
 
insert into ##gathersummary
exec    (@gather) 
 
select * from ##gathersummary where [file_name] like '%MyWildCardSearch%'


```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

