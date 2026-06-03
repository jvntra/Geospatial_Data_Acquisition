# $Geospatial\ Data\ Acquisition:\ Colorado\ School\ Board\ Districts$

## $\ Task\ Objective\ and\ Scope$
The objective of this project is to design a geospatial dataset of colorado school board districts that can support civic research, analysis and decision making. School board districts are an important unit of local governance that influence electoral representation, educational administration, and the distribution of public resources. Because authoritative and up-to-date district boundaries are often disjointed along multiple sources and are not consistently available in readily usable formats, the aim of this project focuses on identifying, evaluating, and integrating reliable geospatial data sources into a structured dataset. Given the time constraints of the assessment, the scope is limited to constructing a representative sample of Colorado school board districts rather than a complete statewide inventory, while documenting the methodology, source evaluation process, data modeling, and quality assurance considerations required to support a scalable statewide solution. 

## $1.)\ Data\ Modeling$

My proposition is to source data for two related layers or tables. Upon a review of public data resources (citations/references), ive learned that "school board district" can refer to multiple geographic levels. Complexity arises because governance boundaries and election boundaries are not always the same. Because parent school districts are likely to not share the same geography as board-member districts (subdistricts), modeling subdistricts separately avoids flattening the complexities of civic geography into one ambiguous polygon. 

The tables would have the following schema

#### $a.)\ Parent\ School\ district\ table:$

`district_id`  
`district_name`  
`district_type`  
`state`  
`county_or_counties`  
`source_name`  
`source_url`  
`source_type`  
`effective_date`  
`last_verified_date`  
`geometry`  
`geometry_quality_score`  
`notes`  

#### $b.)\ Board member\ / director, subdistrict\ table.$

`subdistrict_id`  
`parent_district_id`  
`subdistrict_name`  
`seat_number_or_area`  
`election_method`  
`source_name`  
`source_url`  
`effective_date`  
`geometry`  
`notes`  

#### $2.)\ Source\ Strategy$

| Source | Coverage | Geometry format | Freshness | Authority | Limitations | Decision |
| ------ | -------- | --------------- | --------- | --------- | ----------- | -------- |
| CDPHE Open Data | 178 districts | .shp  | updated 2/5/26 | Census-Data derived |  |  |
| CO State Department of Local Affairs| 178 districts | .shp | updated 8/2024 |  | | |
| CO Dept. of Ed. school - School district maps | School districts | n/a | updated 2/12/19 | general reference | imprecise | useful for comparison & boundary changes, not authoritative. |
| CO Dept. of Ed. school - Admin county maps| Admin county maps | n/a | updated 2/12/19 | general reference | imprecise | useful for comparison & boundary changes, not authoritative. |

