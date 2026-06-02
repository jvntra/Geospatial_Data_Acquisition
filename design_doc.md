# Geospatial Data Acquisition: Colorado School Board Districts

#### $Task Objective and Scope$
The objective of this project is to design a geospatial dataset of colorado school board districts that can support civic research, analysis and operational decision making. School board districts are an important unit of local governance, influencing electoral representation, educational administration, and the distribution of public resources. Because authoritative and up-to-date district boundaries are often fragmented across multiple sources and are not consistently available in readily usable formats, this project focuses on identifying, evaluating, and integrating reliable geospatial data sources into a structured dataset. Given the time constraints of the assessment, the scope is limited to constructing a representative sample of Colorado school board districts rather than a complete statewide inventory, while documenting the methodology, source evaluation process, data modeling, and quality assurance considerations required to support a scalable statewide solution. 

## $1.)\ Data\ Modeling$

My proposition is to source data for two related layers or tables because parent school districts are likely to not share the same geography as board-member districts (subdistricts). Modeling subdistricts separately avoids flattening the complexities of civic geography into one ambiguous polygon. 

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
