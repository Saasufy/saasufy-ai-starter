# Saasufy Admin HTTP API to update the schema of the service

The goal of this API is to allow you to define the data schema of your Saasufy service. It provides flexible endpoints which can be used to construct and update the schema.

You will need to substitute the `Bearer` token in these examples with the one specified inside the `.saasufy-api-key` file.
If the `.saasufy-api-key` file does not exist, it should be created and you should paste your HTTP API secret key inside.
This secret key can be created from your saasufy.com control panel `Dashboard -> Authentication`; under the `Admin HTTP API credentials` section.

This guide shows examples for the 'Model' collection. A model represents a collection type (like a database table) which is available to hold data within the user's Saasufy service (once deployed). The same URL endpoints can be used for any collection type; just substitute the Model collection name and the ID part of the URL as required.

Supported collections which can be used to construct the schema via this HTTP API are:

- Model
- ModelField
- ModelView
- ModelIndex

## Model

### Get a specific model by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/Model/5451d760-ac18-4873-a675-126b7da558a5'
```

### Get a list of all existing models currently available in my Saasufy service

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/Model?view=accountAlphabeticalView'
```

### Add a new model for use in my Saasufy service

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPOST 'https://saasufy.com/api/Model' -d '{"name": "Bar", "position": 2}'
```

### Update a specific model by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPUT 'https://saasufy.com/api/Model/4ace21c0-4513-4d19-93ff-23df62621c62' -d '{"name": "Thing"}'
```

### Delete a specific model by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XDELETE 'https://saasufy.com/api/Model/4ace21c0-4513-4d19-93ff-23df62621c62'
```

## ModelField

### Get a specific model field by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/ModelField/a1b2c3d4-e5f6-4789-a1b2-c3d4e5f67890'
```

### Get a list of all fields for a specific model

```bash
curl -g -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/ModelField?view=accountModelAlphabeticalView&viewParams[modelId]=e95eae22-9dee-464c-8d58-a358049e13d8'
```

### Add a new field to a model

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPOST 'https://saasufy.com/api/ModelField' -d '{\"modelId\": \"e95eae22-9dee-464c-8d58-a358049e13d8\", \"name\": \"email\", \"type\": \"string\", \"required\": true, \"email\": true, \"position\": 1}'
```

### Update a specific model field by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPUT 'https://saasufy.com/api/ModelField/a1b2c3d4-e5f6-4789-a1b2-c3d4e5f67890' -d '{\"required\": false, \"allowNull\": true}'
```

### Delete a specific model field by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XDELETE 'https://saasufy.com/api/ModelField/a1b2c3d4-e5f6-4789-a1b2-c3d4e5f67890'
```

## ModelView

### Get a specific model view by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/ModelView/b2c3d4e5-f6a7-4890-b2c3-d4e5f6a78901'
```

### Get a list of all views for a specific model

```bash
curl -g -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/ModelView?view=accountModelAlphabeticalView&viewParams[modelId]=e95eae22-9dee-464c-8d58-a358049e13d8'
```

### Add a new view to a model

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPOST 'https://saasufy.com/api/ModelView' -d '{\"modelId\": \"e95eae22-9dee-464c-8d58-a358049e13d8\", \"name\": \"activeUsersView\", \"paramFields\": \"status\", \"primaryFields\": \"status\", \"affectingFields\": \"createdAt\", \"transformOrderByField\": \"createdAt\", \"transformOrderByDesc\": true}'
```

### Update a specific model view by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPUT 'https://saasufy.com/api/ModelView/b2c3d4e5-f6a7-4890-b2c3-d4e5f6a78901' -d '{\"transformOrderByDesc\": false, \"disableRealtime\": true}'
```

### Delete a specific model view by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XDELETE 'https://saasufy.com/api/ModelView/b2c3d4e5-f6a7-4890-b2c3-d4e5f6a78901'
```

## ModelIndex

### Get a specific model index by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/ModelIndex/c3d4e5f6-a7b8-4901-c3d4-e5f6a7b89012'
```

### Get a list of all indexes for a specific model

```bash
curl -g -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/api/ModelIndex?view=accountModelAlphabeticalView&viewParams[modelId]=e95eae22-9dee-464c-8d58-a358049e13d8'
```

### Add a new index to a model

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPOST 'https://saasufy.com/api/ModelIndex' -d '{\"modelId\": \"e95eae22-9dee-464c-8d58-a358049e13d8\", \"name\": \"emailIndex\", \"fields\": \"email\"}'
```

### Update a specific model index by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPUT 'https://saasufy.com/api/ModelIndex/c3d4e5f6-a7b8-4901-c3d4-e5f6a7b89012' -d '{\"maxCardinality\": 1000}'
```

### Delete a specific model index by ID

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XDELETE 'https://saasufy.com/api/ModelIndex/c3d4e5f6-a7b8-4901-c3d4-e5f6a7b89012'
```

-------

# Saasufy Service HTTP API to update the data within the service

This shows examples with a 'Message' model; the same API can be used for any other model name; just substitute the Message model name and the ID part of the URL as required. You can read the schema using the `Admin HTTP API` above to understand which fields (`ModelField`) are supported and what constraints are imposed on each Model type.

### Get a record from a model collection

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/sid8006/api/Message/9fed41a0-caae-4620-8ed1-931a3b388fa3'
```

### Get a list of records from a model collection filtered according to the specified view

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XGET 'https://saasufy.com/sid8006/api/Message?view=defaultView'
```

### Add a record to a model collection

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPOST 'https://saasufy.com/sid8006/api/Message' -d '{"message": "Hello there.", "from": "Alice"}'
```

### Update a record from a model collection

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -H "Content-Type: application/json" -XPUT 'https://saasufy.com/sid8006/api/Message/9124ac66-a493-4092-8cbd-3f2c8dd8d65e' -d '{"from": "Steve"}'
```

### Delete a record from a model collection

```bash
curl -H "Authorization:Bearer 7dc295ef916ba6621e2e56d0ce46a8c2866aa820c538119d5b7de292c4197147" -XDELETE 'https://saasufy.com/sid8006/api/Message/9124ac66-a493-4092-8cbd-3f2c8dd8d65e'
```
