## Splunk
Notes and useful queries

### Changing times – variable = expression
- timestamp=strftime(_time,"%d %B, %I:%M %p")

### Lookup
index="_internal"  log_level=* 
- | lookup test.csv log_level OUTPUT colname2
- | table log_level colname2
- For the lookup it is <lookup table> <field to match can be renamed to match> OUTPUT <what you want to take from the lookup table>

### Create Map
- | inputlookup geo_us_states
- | join featureId [search host="homework" state=* usr=* | rename state AS state_code | lookup geo_attr_us_states.csv state_code OUTPUT   -state_name AS featureId]
- | stats count(usr) BY featureId
- | geom geo_us_states featureIdField=featureId
- Need to make the geographic match from the inputlookup table. In that, the states are called featureId, so need to rename my column that so it can join up. For the join, just specify the name of the column to join on and then do the table or create a subquery as above

  - geo_attr_countries File 
  - geo_attr_us_states File 
  - geo_countries geo
  - geo_sf_neighborhoods geo
  - geo_us_states geo 

### Search Substring
- Search a substring in an evalution field
  - | eval flag =if(like(eventDescription, "%clicked%"),1,0)
  
- Filter a search based on substring  
  - | where like(eventDescription, "%clicked%")

### Use lists like SQL in
- Search command supports IN operator
  - sourcetype=xyz status IN (100, 102, 103)

- Eval and where commands support in function
  - | where in(status,"222","333","444","555")

### Nice summary
- sourcetype="alm-spans" | head 1000 | fieldSummary


### Null
- isnull(xxx_field), then...
