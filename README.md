# ETL Project with Subway USA Stores DataSet 🥪

ETL structure that: 🧾 extracts data by Google API --> 🗄 Saving and connecting data process with ☁️ Google Cloud --> 📊 Dashboard to take 👔 decisions with Looker Studio 🖥



- 🗃️ Get Data from 🗺️ Google Maps API 

- 📡 Big Query Integration 

- 📈 Looker Studio Conection:

https://datastudio.google.com/reporting/6b018d61-0515-4142-94c0-7c544b7b58bc

##

1. Access to the Google Maps API with the latitude and longitude of each store, then we get a "JSON" with all data available about business near 2 km to Subways stores.  
2. Use the "Business" Keyword to find all the relevant data about differents types of stores near to our focus in subway.
3. After extracting all data,  created a "Python" function that normalizes and gives us a "dataframe" 


