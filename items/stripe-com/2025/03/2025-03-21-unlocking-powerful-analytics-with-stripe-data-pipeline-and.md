---
title: Unlocking powerful analytics with Stripe Data Pipeline and Google BigQuery
link: https://stripe.dev/blog/unlock-powerful-analytics-stripe-data-pipeline-bigquery
source: stripe-com
published: 2025-03-21T00:00:00Z
updated: 2025-03-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Sushant Jain
summary: Discover how to integrate Stripe Data Pipeline with Google Cloud Storage and BigQuery for powerful analytics. This guide walks you through setting up automated data transfers, optimizing reporting, and unlocking valuable insights to enhance your business decisions.
content: extracted
html: 2025-03-21-unlocking-powerful-analytics-with-stripe-data-pipeline-and.html
remote_preview:
  url: https://stripe.dev/og/blog/unlock-powerful-analytics-stripe-data-pipeline-bigquery.png
  alt: 'Stripe Developer Blog: Unlocking powerful analytics with Stripe Data Pipeline and Google BigQuery'
extra:
  content:interactive: true
---

As businesses scale, understanding revenue data is critical for making informed decisions and deriving valuable insights. [Stripe Data Pipeline](https://stripe.com/en/data-pipeline) (SDP) simplifies the process of exporting modeled revenue data into your data storage of choice.

In this blog post, we'll walk through how to integrate your Stripe Data Pipeline set up with [Google Cloud Storage (GCS)](https://cloud.google.com/storage/docs) to run analytics in [BigQuery](https://cloud.google.com/bigquery/docs). You'll learn how to seamlessly move data from GCS to BigQuery, and how to automate this process for continuous reporting and analysis.

![](https://stripe.dev/images/unlock-powerful-analytics-stripe-data-pipeline-bigquery/1.png)

## Why use Stripe Data Pipeline for analytics?

1. **Automate data delivery at scale**: Stripe Data Pipeline makes it easier for businesses to export their data from Stripe. It allows businesses to automatically export their data, including transactions, invoices, subscriptions, and payments, directly to a Google Cloud Storage (GCS) bucket. This means less manual data handling and fewer errors, providing a smooth flow of data for analytics.

2. **Do more with your data**: Speed up your financial close process and unlock new insight by centralizing your Stripe data with our business data. Using SDP means that multiple teams are using the same source of truth for accounting, product, growth, and different use cases.

3. **Direct and secure integration**: Send your data directly to Google Cloud Storage without involving a third party to handle your sensitive revenue data.

4. **Avoid data outages and delays**: Offload delivery and pipeline maintenance from your data engineering teams. Businesses can access fresh data available for analysis frequently, ensuring that you have timely insights to make informed decisions while still benefiting from a reliable and automated pipeline. For more details, read more about freshness expectations [in the documentation](https://docs.stripe.com/stripe-data/available-data).

## Integrating Stripe Data Pipeline with Google Cloud Storage and BigQuery

At a high level, these are the steps you must follow:

1. Set up the Stripe Data Pipeline in the Stripe Dashboard.
2. Use Google Cloud Run Job to set up the transfer pipeline.
3. Test the integration by triggering the job.
4. Verify that data is present in BigQuery.
5. Set up a Scheduler to run the recurring job.

Below is a detailed description for each of these steps with code examples on how to set up the whole pipeline.

### Prerequisites

- Administrator access for your Stripe account.
- Access to [Google Cloud Platform](https://cloud.google.com/docs) account and a [Project](https://developers.google.com/workspace/guides/create-project).
- A [BigQuery Dataset](https://cloud.google.com/bigquery/docs/datasets) for running the analytics and queries.

### Step 1: Set up Stripe Data Pipeline.

Navigate to the Stripe Dashboard, and choose *Reporting > Data Management*. Follow the steps in the self-serve onboarding. Once configured, you have access to a structured set of files organized by time. The first data load can take up to 12 hours.

### Step 2: Use a Google Cloud Run Job to set up the transfer pipeline.

1. Create an empty repository on your local machine. Add the code snippets shared below to create a data pipeline using Python. Later you will deploy it as a recurring job using [Google Cloud CLI](https://cloud.google.com/sdk/docs).

2. Finding the latest Data Snapshot: Stripe Data Pipeline maintains a file `data_load_times.json` which has the information for all the tables that were added to GCS and their latest folder path:

```
{
 "tableLoadTimes": [
   {
     "tableName": "connected_account_external_account_bank_accounts",
     "rundate": "2025031106",
     "dataset": "connect-coreapi",
     "mode": "LIVEMODE",
     "succeededAt": 1741693893927,
     "path": "2025031106/livemode/connected_account_external_account_bank_accounts"
   },
   {
     "tableName": "connected_account_products",
     "rundate": "2025031106",
     "dataset": "connect-coreapi",
     "mode": "LIVEMODE",
     "succeededAt": 1741693894217,
     "path": "2025031106/livemode/connected_account_products"
   },
   .
   .
   .
  ]
}
```

Using the [Python Client for Google Cloud Storage](https://cloud.google.com/python/docs/reference/storage/latest), you can load this file and convert it to a Python dictionary to find the paths of the data to be loaded to BigQuery.

```
from google.cloud import storage, bigquery
import json
import concurrent.futures

# Initialize clients for GCS and BigQuery
storage_client = storage.Client()
bigquery_client = bigquery.Client()

# Set up your project, bucket and BigQuery dataset
bucket_name = "stripe-data"  # Your GCS bucket name
dataset_id = "stripe_data"  # BigQuery dataset name
project_id = "my-test-proj-12345" # Your GCP project ID
mode = "LIVEMODE"  # The mode for which the data is being loaded

# Function to get the latest folder based on the data_load_times.json file
def get_latest_load_time_dict():
   # Load the JSON file from GCS
   bucket = storage_client.get_bucket(bucket_name)
   blob = bucket.blob('data_load_times.json')
   data_load_times_content = blob.download_as_text()

   # Parse the JSON data
   data_load_times = json.loads(data_load_times_content)
   print("Latest folder found")
   return data_load_times
```

3. **Listing Data Files in GCS**: Each table shared by SDP is loaded as a folder. You must iterate over each table and list all the Parquet files present in the folder to be loaded to BigQuery:

```
# Function to list all tables folders and their .parquet files
def list_tables_and_files(data_load_times):
   # Create a dictionary of table names and their corresponding folders
   table_folders_dict = {}
   for table_data in data_load_times["tableLoadTimes"]:
       if table_data.get("mode") == mode:
           table_folders_dict[table_data["tableName"]] = table_data['path']
   print(f"Table folders dict loaded: Found {len(table_folders_dict)} tables")
   # Now for each table folder, list the .parquet files
   table_files_dict = {}
   for table_name, table_folder in table_folders_dict.items():
       parquet_files = []
       blobs = storage_client.list_blobs(bucket_name, prefix=table_folder)
      
       for blob in blobs:
           if blob.name.endswith(".parquet"):
               parquet_files.append(blob.name)
      
       table_files_dict[table_name] = parquet_files
  
   print("Table files dict loaded")
   return table_files_dict
```

4. **Loading Parquet Files into BigQuery**: Once you have a list of your data tables in GCS, you must load the Parquet files into BigQuery. Using the `google-cloud-bigquery` client, you can configure the load job to write the data directly into BigQuery tables. You can optimize the load time by running multiple of these jobs in parallel.

```
# Function to load the data from GCS into BigQuery
def load_to_bigquery(gcs_paths, table_name):
   try:
       # Define the BigQuery load job configuration for Parquet files
       job_config = bigquery.LoadJobConfig(
           source_format=bigquery.SourceFormat.PARQUET,
           write_disposition=bigquery.WriteDisposition.WRITE_TRUNCATE,  # Always truncate the table before loading
           create_disposition=bigquery.CreateDisposition.CREATE_IF_NEEDED,  # Create table if it doesn't exist
       )

       # Map the file path to full URI
       uri_list = [f"gs://{bucket_name}/{gcs_path}" for gcs_path in gcs_paths]
     
       # Load data into BigQuery
       load_job = bigquery_client.load_table_from_uri(
           uri_list,
           f"{project_id}.{dataset_id}.{table_name}",
           job_config=job_config,
       )

       # Wait for the load job to complete
       load_job.result()  # This will wait for the load job to finish

       # Check if the job completed successfully
       if load_job.error_result is None:
           print(f"Loaded data into BigQuery table {dataset_id}.{table_name}")
       else:
           raise Exception(f"{load_job.error_result}")
      
   except Exception as e:
       print(f"Error occurred while loading data into BigQuery table {dataset_id}.{table_name} : {e}")
       raise e

# Use ThreadPoolExecutor to load data in parallel
def load_tables_in_parallel(table_files):
   error_dict = {}
   error_count = 0

   # Use ThreadPoolExecutor for parallel loading
   with concurrent.futures.ThreadPoolExecutor() as executor:
       # Submit all load tasks to the executor
       future_to_table = {executor.submit(load_to_bigquery, gcs_paths, table_name): table_name for table_name, gcs_paths in table_files.items()}

       # Collect exceptions if any occur during execution
       for future in concurrent.futures.as_completed(future_to_table):
           table_name = future_to_table[future]
           try:
               future.result()  # This will raise any exception that occurred in load_to_bigquery
           except Exception as e:
               error_dict[table_name] = str(e)
               error_count += 1

   return error_dict, error_count
```

5. **Putting it all together**: Finally, you must stitch together all these functions to find the latest snapshot, read the data and insert to BigQuery:

```
# Cloud Function main entry point
def load_latest_data():
   try:
       # Get the latest folder based on the data_load_times.json file
       latest_load_time_dict = get_latest_load_time_dict()
      
       # List all tables and their .parquet files under the livemode folder
       table_files = list_tables_and_files(latest_load_time_dict)
      
       # Load the data into BigQuery in parallel
       error_dict,error_count = load_tables_in_parallel(table_files)

       if error_count > 0:
           print(f"Data loading completed with {error_count} errors. Error details: {error_dict}")
       else:
           print("Data loading completed successfully.")

   except Exception as e:
       print(f"Error processing request: {e}")

if __name__ == "__main__":
   load_latest_data()
```

Also add a `requirement.txt` file with the needed dependencies:

```
google-cloud-storage
google-cloud-bigquery
```

And a file named “Procfile” without any extension:

```
web: python3 main.py
```

### Step 3: Test the integration.

The script is now ready to be tested. You are going to use Google Cloud Run Job to run the script for inserting data to BigQuery. Make sure you have [Gcloud CLI](https://cloud.google.com/sdk/gcloud) installed and logged in using this [guide](https://cloud.google.com/sdk/docs/install) before continuing.

From the same folder where you created this script, run the following command:

```
gcloud run jobs deploy job-sdp-dataload \
   --source . \
   --max-retries 5
```

Choose the preferred region and select `[Y]` for creating an artifact registry. This creates the Docker image for the script, uploads it to Google Artifact Registry, and deploys the Cloud Run job.

You can then run the job using the Gcloud Console or by running the command:

```
gcloud run jobs execute job-sdp-dataload
```

### Step 4: Verify the data.

You can now verify the data by visiting the BigQuery Console and checking that all tables have been created under your specified DataSet. Since you use `WRITE_TRUNCATE` while writing the tables to BigQuery, you only see the latest snapshot of data in BigQuery.

This is how the tables appear once they are loaded to BigQuery:

![](https://stripe.dev/images/unlock-powerful-analytics-stripe-data-pipeline-bigquery/2.png)

### Step 5: Schedule the Google Cloud Run Job.

Once the integration is running, you can schedule it to run every 6 hours to ensure the latest data is in BigQuery. To do this, visit the *Add Trigger* page on the created Cloud Run Job page.

![](https://stripe.dev/images/unlock-powerful-analytics-stripe-data-pipeline-bigquery/3.png)

## ![](https://stripe.dev/images/unlock-powerful-analytics-stripe-data-pipeline-bigquery/4.png)

## Conclusion

Integrating Stripe Data Pipeline (SDP) with Google Cloud Storage and BigQuery unlocks powerful analytics capabilities for Stripe users. The seamless integration between SDP and GCS allows businesses to easily export their payment data, while BigQuery offers the performance and scalability needed for querying and analyzing large datasets.

Whether you are looking to build automated data pipelines, perform real-time analytics, or unlock business insights, using SDP with Google Cloud provides a flexible, cost-effective, and scalable solution.

By using Stripe Data Pipeline, businesses can analyze their Stripe data effortlessly and gain a competitive edge through timely and data-driven decisions.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
