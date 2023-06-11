# ETL Project with Subway USA Stores DataSet 🥪

ETL structure that: 🧾 extracts data by Google API --> 🗄 Saving and connecting data process with ☁️ Google Cloud --> 📊 Dashboard to take 👔 decisions with Looker Studio 🖥



- 🗃️ Get Data from 🗺️ Google Maps API 

- 📡 Big Query Integration 

- 📈 Looker Studio Conection:

https://datastudio.google.com/reporting/6b018d61-0515-4142-94c0-7c544b7b58bc

##

1. Access to the Google Maps API with the latitude and longitude of each store, then we get a `JSON` with all data available about business near 2 km to Subways stores.  
2. Use the `Business` Keyword to find all the relevant data about differents types of stores near to Subway.
3. After extracting all data,  created a `Python` function that normalizes and gives us a `dataframe`. 
4. The final dataframe will be saved in a `Bucket` *etl_raw_data* inside the fold before being created. This allows the use of the data without overwriting the already extracted.

##

``` ruby
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
import gcsfs

from googleplaces import GooglePlaces, types, lang
YOUR_API_KEY = '*****************************'

google_places = GooglePlaces(YOUR_API_KEY)

from google.cloud import storage
import os
os.environ["GOOGLE_APPLICATION_CREDENTIALS"]="etl-project-370513-***********.json"

df = pd.read_csv('gs://provided-table/subway.csv')
```
#Testing Keyword to execute the call all at once, due to the charge time being long and the Free version API being limited 
``` ruby
df_sample1 = df[df["city"]=="Alexandria"]
df_sample1.to_csv('df_sample1.csv', index = False)
```
#Extract function from API by Lat and Long, to then save in a data frame.
``` ruby
def search_locations(data):

    locations_final = pd.DataFrame(columns=['business_status', 'icon', 'icon_background_color',
    'icon_mask_base_uri', 'name', 'photos', 'place_id', 'rating',
    'reference', 'scope', 'types', 'user_ratings_total', 'vicinity',
    'geometry.location.lat', 'geometry.location.lng',
    'geometry.viewport.northeast.lat', 'geometry.viewport.northeast.lng',
    'geometry.viewport.southwest.lat', 'geometry.viewport.southwest.lng',
    'opening_hours.open_now', 'plus_code.compound_code',
    'plus_code.global_code', 'lat_ref', 'long_ref'])

    for i in range(len(data)):

        query_result = google_places.nearby_search(
            lat_lng={'lat': data['latitude'].iloc[i], 'lng': data['longitude'].iloc[i]}, 
            keyword='business',
            radius=2000, 
            types=[types.TYPE_FOOD])

        result = query_result.raw_response

        locations = pd.json_normalize(result, record_path =['results'])
        locations["lat_ref"]=data['latitude'].iloc[i]
        locations["long_ref"]=data['longitude'].iloc[i]

        locations_final=pd.concat([locations,locations_final])

    return(locations_final)


result_api_google = search_locations(df)

result_api_google.shape

```
#Saving  in cvs format
``` ruby
result_api_google.to_csv('result_api_google.csv', index = False)
``` 
#Saving in Google Cloud Storage
``` ruby
bucket_name = 'etl_raw_data' 
local_path = 'result_api_google.csv' 
blob_path = datetime.today().strftime('%Y-%m-%d') + '/data/' + local_path

bucket = storage.Client(project='etl-project-370513').bucket(bucket_name)
blob = bucket.blob(blob_path)
blob.upload_from_filename(local_path)
```





