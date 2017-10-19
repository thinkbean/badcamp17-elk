# Configure Index Patterns & View Data in Kibana

Index patterns allow you analyze one or more indices in Kibana.

Note: Index patterns in Kibana does not affect your data directly. It is only used by Kibana to target the data
you want to analyze.

The naming convention we use for access logs is: `access-[project]-[environment]`.

For example:
```
access-foo-prod
access-bar-prod
```

Defining an index pattern of `access-*` will allow us to query across all indices that begin with `access-`.

## Create an index pattern

Important! - At least one index which matches the index pattern must exist. Be sure you have sent data to
Elasticsearch before creating an index pattern.

1. Go to the index pattern management screen by clicking `Managment -> Index Patterns`.

http://192.168.56.101:5601/app/kibana#/management/kibana/index

If it's your first time here, you will be on the add screen automatically.

Otherwise, you will need to click `Create Index Pattern`.

2. Enter `access-*` as the index pattern.

3. Select `@timestamp` as the `Time Filter field name`.

4. Click `Create`.

## Viewing your data

Click the `Discover` tab and select the index pattern you just created.

The time filter defaults to the last 15 minutes, so if there are no logs in this time range, there will be no data.

If you don't see data, modify the time selector in the upper right.

The list of data isn't organized by default, but you can add some sanity by clicking `add` next to 
the `Available Fields` on the left.

If the view you create is something you want to use again, you can click `Save` in the upper right.
