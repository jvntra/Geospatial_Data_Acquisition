# $Geospatial\ Data\ Acquisition:\ Colorado\ School\ Board\ Districts$

## $\ Task\ Objective\ and\ Scope$
The objective of this project is to design a geospatial dataset of colorado school board districts that can support civic research, analysis and decision making. School board districts are an important unit of local governance that influence electoral representation, educational administration, and the distribution of public resources. Because authoritative and up-to-date district boundaries are often disjointed along multiple sources and are not consistently available in readily usable formats, the aim of this project focuses on identifying, evaluating, and integrating reliable geospatial data sources into a structured dataset. Given the time constraints of the assessment, the scope is limited to constructing a representative sample of Colorado school board districts rather than a complete statewide inventory, while documenting the methodology, source evaluation process, data modeling, and quality assurance considerations required to support a scalable statewide solution. 

## $1.)\ Data\ Modeling$

My proposition is to source data for two related layers or tables. Upon a review of public data resources, ive learned that "school board district" can refer to multiple geographic levels. Complexity arises because governance boundaries and election boundaries are not always the same. Because parent school districts are likely to not share the same geography as board-member districts (subdistricts), modeling subdistricts separately avoids flattening the complexities of civic geography into one ambiguous polygon. 

The tables would have the following schema:

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


overall this is a table that concerns the overall administrative geography and contains the boundaries that define which students attend the district, taxing jurisdiction, funding and overall school governance.

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

This is a table that would differ in that a school district polygon and a subdistrict or board member election polygon are not necessarily the same. Different subdistricts also exist such as the unified school district, elementary school district, high school district and boards of cooperative educational services (BOCES) which provide services across multiple districs and are not usually school board districts and should not be confused with district polygons but are still useful to include if there are specialized analyses downstream. 

As mentioned in the Assessment instructions, individual board member seats within a district are rarely included at all so it is important to move through the data modeling process asking "what political geography actually exists?". With that it'd be crucial to design a model that entertains or includes different boundaries based on what information actually exists. Namely, School District boundaries, Subdistrict/ board director boundaries and boundaries where board members are elected at-large (boundaries that can possibly extend across the first two.)

Overall, school district boundaries and school board election geographies should be modeled as separate but related entities because many districts elect board members from director districts that do not necessarily correspond to the district boundary itself, while other districts use at-large elections and have no subdistrict geometry at all

#### $2.)\ Source\ Evaluation\ &\ Strategy$

Evaluation table:

| Source | Coverage | Geometry format | Freshness | Authority | Limitations | Decision |
| ------ | -------- | --------------- | --------- | --------- | ----------- | -------- |
| CDPHE Open Data | 178 School district codes and name | .shp  | updated 2/5/26, Medium | Census derived, High | intended for visualization, boundary precision may be limited | Validation source: use to verify district names, coverage and boundary reasonability, not primary geometry source |
| CO State Department of Local Affairs| 178 school board districts | .shp | Medium-High | Medium-High | Census-derived, update frequency and methodology require verification | Primary geometry source (initial): machine readable polygons with broad statewide coverage, can be validated against more authoritative district level sources|
| CO Dept. of Ed. School & County district maps | School district boundaries | n/a | updated 2/12/19 | general reference | imprecise | useful for comparison & boundary changes, imprecise |
| County GIS portals | county-level |  | High | High | coverage may be incomplete for multi-county districts, maintenance frameworks vary by county | **Supplemental Source.** use to fill gaps or cross-check district boundaries when district level data is unavailable |
| Board election maps (pdf) |  | n/a | High | Very High | Not machine readable, may require digitizing | Primary Source for subdistricts. Use when investigating board director districts since these structures are often unavailable elsewhere |
| Census TIGER School District Boundaries | school district level | .shp | Medium | High | May lag local changes and generally exclude board member election districts| **Baseline Source** useful for establishing statewide coverage and comparison but not sufficient on its own |
|State statutes/ Legal boundary descriptions|county boundaries|n/a|High|Very High|Difficult and time consuming to translate into geometry|**Escalation Source.** use only when geometry is unavailable elsewhere or when resolving conflicting boundaries.|


#### $3.)\ Geometry\ Acquisition\ and\ Approach$

I would detail the acquisition approach based on the source type.

For direct shapefile/GeoJSON/APIs:
- load Geopandas
- standardize/verify CRS
- validate geometry
- normalize attributes

For PDF map sources:
- use as a reference
- search for underlying GIS source
- if needed, georeference or digitize manually
- Mark as approximate

For Legal description sources:
- parse boundary descriptions
- reconstruct using parcels, roads, precints and municipal/county boundaries
- note confidence and assumptions

No clean source:
- use parent district boundary only
- flag missing subdistrict geometry
- document source gap

#### $4.)\ Quality\ Assessment$

to assess the quality of the sources I would check:
- CRS is known and consistent
- geometry is valid and non-empty
- area values are reasonable
- polygons fall within Colorado
- overlaps/gaps checked within same district layer
- district names match with source layer
- spot check against official maps
- record source date and effective date

what would unsettle me when doing quality assessment would be:
- old census derived boundaries
- PDF maps with unclear scale
- if districts recently redistricted
- subdistricts described visually but not downloadable (as in the case of the douglas county subdistrict map)
- geometry loaded successfully but source lacks effective date. 

#### $5.)\ Tradeoffs\ and\ Open\ Questions $

Given the complexity of collecting GIS data from disparate sources, the tradeoffs I would consider when making decisions on how to compile the data would involve:
- complexity vs quality
- Parent districts vs board subdistricts
- official but not machine readable PDFs vs machine readable but antiquated census files (perhaps there is a way to use machine learning to streamline digitization)
- Automated extraction vs manual verification