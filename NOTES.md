# Notes for running this tutorial

## Authentication

### Authentication using username/password

Fails we cannot login with personal account. Only work or school is accepted. Why?

So we cannot test this method. Trying others.


### Authentication using Appkey, AAD (Azure Active Directory)

https://docs.microsoft.com/en-us/azure/data-explorer/provision-azure-ad-app
https://stackoverflow.com/questions/56334954/how-to-properly-authenticate-kusto-using-a-python-client


#### create app in azure portal

- Azure Portal
  - Azure Active Directory: Default Directory
    - Tenant: 3fd515b5-fc97-41f0-a988-acc9b42b6e7d
    - App registrations: try-azure-kusto-python-aad-app
      - Appkey: 40173ed7-1f2e-4ad7-8cd8-81530be01dd4
      - Certificates & secrets:
        - secret key for azure kusto python
          - Client secret value: # please see secret.zip
          - Client secret key: # please see secret.zip
      - API permissions:
        - Azure Data Explorer
          - user-impersonation


#### add permission to adx database

1. UI
- Azure Portal
  - Azure Data Explorer
    - cluster: tryazurekustopython
      - database: try-azure-kusto-python-database
        - permissions
          - Add: try-azure-kusto-python-aad-app

2. KQL web UI

This did not work for me. Says command syntax incorrect.

```
.add database <DatabaseName> viewers ('<ApplicationId>') '<Notes>'
# For example:
# .add database Logs viewers ('aadapp=f778b387-ba15-437f-a69e-ed9c9225278b') 'Azure Data Explorer App Registration'
```

#### connect

```
KustoConnectionStringBuilder.with_aad_application_key_authentication(
    cluster_url, os.environ.get("APP_ID"), os.environ.get("APP_KEY"), os.environ.get("APP_TENANT")
)
```