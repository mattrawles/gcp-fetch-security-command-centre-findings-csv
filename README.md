# Fetch GCP Security Centre Findings

Small script (copy of https://github.com/dominikjaeckle/aws-fetch-security-hub-findings ) to fetch gcp security command centre findings based on defined account_ids and filters. The script reads a configuration file and fetches the security findings based on pre-defined account_ids and filters, which are to be defined in the settings_gcp.yaml file. 

Requires gcloud CLI and activated login or service account (download the key file from the gcpconsole, e.g. as "gcpcreds.json" and reference in the cli command below)

```bash
For example

Please run:
  $ gcloud auth login
to obtain new credentials.

For service account, please activate it first:
  $ gcloud auth activate-service-account ACCOUNT --key-file=gcpcreds.json
```


Script is running this command and then parsing the json outpout to html and csv.
```bash
GCP_ACCOUNT=my-account-id
GCP_FILTER="state=\"ACTIVE\""
gcloud scc findings list projects/$GCP_ACCOUNT --filter=$GCP_FILTER
```

The script expects gcp account credentials in a file gcpcreds.json (see example).

Modified to support a csv output and adapted for gcp gcloud from https://github.com/dominikjaeckle orginal aws script.

## Prerequisites
If you have not done so, setup gcloud cli and configure a service account similar to those required for cloudsploit or similar tools ( https://github.com/aquasecurity/cloudsploit/blob/master/docs/gcp.md)

Also install the requirements:
```bash
$ pip install -r requirements.txt
```

## Configuration
Find the configuration in the **settings_gcp.yaml** file.
```yaml
# Configure all accounts your credentials has access to.
accounts:
  - $$account_id1$$
  - $$account_di2$$
  - ...

# filters to be used to filter for security findints
filters:
  - filter_name: state
    value: ACTIVE
    comparison: '='

```

## Fetch the Security Command Centre Findings
Run the following command to fetch the security findings
```bash
$ python3 gcp_fetch_sec_findings.py
```

In the same directory, the script will generate a file called **security_findings_%Y%m%d.html** and a file **security_findings_%Y%m%d.csv**, which can be opened in any browser. 

## Extensions
The basic set of attributes that is extracted from the security hub findings can be extended as per your convinience. So far, the following definition exists using pydantic:

```python
class Finding(BaseModel):
    account_id: str = ''
    created_at: str = ''
    updated_at: str = ''
    compliance_status: str = ''
    category: str = ''
    description: str = ''
    recommendation_text: str = ''
    record_state: str = ''
    severity_label: str = ''
    resource_type: str = ''
    resource_id: str = ''
    resource_details: str = ''
    canonicalName: str = ''
    findingClass: str = ''
```