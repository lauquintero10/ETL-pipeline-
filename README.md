# ETL Project with Subway USA Stores DataSet 🥪

ETL structure that extracts data 🧾 by Google API -> 🗄 Saving and connecting data process with ☁️  Google Cloud -> 📊 Dashboard to take decisions 👔 with Looker Studio 🖥 . 


1 Step: 🗃️ Get Data from 🗺️ Google Maps API 


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


