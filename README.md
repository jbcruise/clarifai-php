# clarifai-php

![Travis Build Status](https://travis-ci.org/darrynten/clarifai-php.svg?branch=master)
![StyleCI Status](https://styleci.io/repos/81687310/shield?branch=master)
[![codecov](https://codecov.io/gh/darrynten/clarifai-php/branch/master/graph/badge.svg)](https://codecov.io/gh/darrynten/clarifai-php)
![Packagist Version](https://img.shields.io/packagist/v/darrynten/clarifai-php.svg)
![MIT License](https://img.shields.io/github/license/darrynten/clarifai-php.svg)

[Clarifai API](https://developer.clarifai.com/docs/) client for PHP

This is a 100% fully unit tested and (mostly) fully featured unofficial PHP
client for Clarifai

> Clarifai is an artificial intelligence company that excels in visual
recognition, solving real-world problems for businesses and developers
alike.

```bash
composer require darrynten/clarifai-php
```

PHP 5.6+

## Basic use

The API is rather simple, and consists of Inputs, Concepts and Models.

### Definitions

#### Inputs

You send inputs (images) to the service and it returns predictions. In
addition to receiving predictions on inputs, you can also 'save' inputs
and their predictions to later search against. You can also 'save' inputs
with concepts to later train your own model.

#### Model

Clarifai provides many different models that 'see' the world differently.
A model contains a group of concepts. A model will only see the concepts
it contains.

There are times when you wish you had a model that sees the world the way
you see it. The API allows you to do this. You can create your own model
and train it with your own images and concepts. Once you train it to see
how you would like it to see, you can then use that model to make
predictions.

You do not need many images to get started. We recommend starting with 10
and adding more as needed.

#### Concepts

Concepts play an important role in creating your own models using your
own concepts. Concepts also help you search for inputs.

When you add a concept to an input, you need to indicate whether the
concept is present in the image or if it is not present.

## Features

This is the basic outline of the project and is a work in progress.

Checkboxes have been placed at each section, please check them off
in this readme when submitting a pull request for the features you
have covered.

### Application base

* Guzzle is used for the communications
* The library has 100% test coverage
* The library supports framework-agnostic caching so you don't have to
worry about which framework your package that uses this package is going
to end up in.

The client is not 100% complete and is a work in progress, details below.

The structure is heavily inspired by [The official JS client](https://github.com/Clarifai/clarifai-javascript)

### Authentication

Access is handled via oauth2.

You would need to initialise the client with your Client ID and Secret.

```php
$this->clarifai = new Clarifai('clientId', 'clientSecret');
```

### Predict

This is a basic library usage example that uses a predict call. The model name is `aaa03c23b3724a16a56b629203edc62c`

```php

include 'vendor/autoload.php';

$clarifai = new \DarrynTen\Clarifai\Clarifai(
    CLIENT_ID,
    CLIENT_SECRET
);

$modelResult = $clarifai->getModelRepository()->predictUrl(
    'https://samples.clarifai.com/metro-north.jpg',
    \DarrynTen\Clarifai\Repository\ModelRepository::GENERAL
);

echo json_encode($modelResult);

```

The response (abridged) would be:

```js
{
  "status":{
     "code":10000,
     "description":"Ok"
  },
  "outputs":[
     {
        "id":"db1b183a95a042d3bd873f8ca69ae2e6",
        "status":{
           "code":10000,
           "description":"Ok"
        },
        "created_at":"2017-02-14T03:18:54.548733Z",
        "model":{
           "name":"general-v1.3",
           "id":"aaa03c23b3724a16a56b629203edc62c",
           "created_at":"2016-03-09T17:11:39.608845Z",
           "app_id":null,
           "output_info":{
              "message":"Show output_info with: GET \/models\/{model_id}\/output_info",
              "type":"concept"
           },
           "model_version":{
              "id":"aa9ca48295b37401f8af92ad1af0d91d",
              "created_at":"2016-07-13T01:19:12.147644Z",
              "status":{
                 "code":21100,
                 "description":"Model trained successfully"
              }
           }
        },
        "input":{
           "id":"db1b183a95a042d3bd873f8ca69ae2e6",
           "data":{
              "image":{
                 "url":"https:\/\/samples.clarifai.com\/metro-north.jpg"
              }
           }
        },
        "data":{
           "concepts":[
              {
                 "id":"ai_HLmqFqBf",
                 "name":"\u043f\u043e\u0435\u0437\u0434",
                 "app_id":null,
                 "value":0.9989112
              },
              // and several others
              {
                 "id":"ai_VSVscs9k",
                 "name":"\u0442\u0435\u0440\u043c\u0438\u043d\u0430\u043b",
                 "app_id":null,
                 "value":0.9230834
              }
           ]
        }
     }
  ]
}
```

This can happen either with an image URL:

```php
    $modelResult = $clarifai->getModelRepository()->predictPath(
        '/user/images/image.png',
        \DarrynTen\Clarifai\Repository\ModelRepository::GENERAL
    );
```

or b64 encoded data:

```php
    $modelResult = $clarifai->getModelRepository()->predictEncoded(
        ENCODED_IMAGE_HASH,
        \DarrynTen\Clarifai\Repository\ModelRepository::GENERAL
    );
```

## Documentation

This will eventually fully mimic the documentation available on the site.
https://developer.clarifai.com/guide

Each section must have a short explaination and some example code like on
the API docs page.

Checked off bits are complete.

- [x] Inputs
  - [x] Add
  - [x] Add with Concepts
  - [x] Add with Custom Metadata
  - [x] Add with Crop
  - [x] Get Inputs
  - [x] Get Input Status
  - [x] Update Input with Concepts
  - [x] Delete Concepts from Input
  - [x] Bulk Update Inputs with Concepts
  - [x] Bulk Delete Concepts from Input List
  - [x] Delete Input by ID
  - [x] Delete Input List
  - [x] Delete All Inpits
- [x] Models
  - [x] Create Model
  - [x] Create Model With Concepts
  - [x] Add Concepts to a Model
  - [x] Remove Concept from Model
  - [x] Update Model Name and Configuration
  - [x] Get Models
  - [x] Get Model by ID
  - [x] Get Model Output Info by ID
  - [x] List Model Versions
  - [x] Get Model Version by ID
  - [x] Get Model Training Inputs
  - [x] Get Model Training Inputs by Version
  - [x] Delete Model
  - [x] Delete Model Version
  - [x] Delete All Models
  - [x] Train Model
  - [x] Predict With Model
- [x] Searches
  - [x] Search Model by Name and Type
  - [x] Search by Predicted Concepts
  - [x] Search by User Supplied Concept
  - [x] Search by Custom Metadata
  - [x] Search by Reverse Image
  - [x] Search Match URL
  - [x] Search by Concept and Prediction
  - [x] Search ANDing
- [x] Pagination
- [x] Patching
  - [x] Merge
  - [x] Remove
  - [x] Overwrite
- [ ] Batch Requests
- [ ] Languages

## Inputs

The API is built around a simple idea. You send inputs (images) to the service
and it returns predictions. In addition to receiving predictions on inputs, you
can also 'save' inputs and their predictions to later search against. You can
also 'save' inputs with concepts to later train your own model.

### Add Inputs

You can add inputs one by one or in bulk. If you do send bulk, you are limited
to sending 128 inputs at a time.

Images can either be publicly accessible URLs or file bytes. If you are sending
file bytes, you must use base64 encoding.

You are encouraged to send inputs with your own id. This will help you later
match the input to your own database. If you do not send an id, one will be
created for you.

#### Add an input using a publicly accessible URL

```php
    $input = new Input();
    $input->setImage('https://samples.clarifai.com/metro-north.jpg')->isUrl();
    $inputResult = $clarifai->getInputRepository()->add($input);
```

#### Add an input using local path to image

```php
    $input = new Input();
    $input->setImage('/samples.clarifai.com/metro-north.jpg')->isPath();
    $inputResult = $clarifai->getInputRepository()->add($input);
```

#### Add an input using bytes

The data must be base64 encoded. When you add a base64 image to our servers, a
copy will be stored and hosted on our servers. If you already have an image
hosting service we recommend using it and adding images via the url parameter.

```php
    $input = new Input();
    $input->setImage(ENCODED_IMAGE_HASH)->isEncoded();
    $inputResult = $clarifai->getInputRepository()->add($input);
```

#### Add multiple inputs with ids

```php
    $input1 = new Input();
    $input1->setImage('https://samples.clarifai.com/metro-north.jpg')->isUrl()->setId('id1');
    $input2 = new Input();
    $input2->setImage('https://samples.clarifai.com/puppy.jpeg')->isUrl()->setId('id2');
    $inputResult = $clarifai->getInputRepository()->add([$input1, $input2]);
```

#### Add inputs with concepts

```php
    $concept = new Concept();
    $concept->setId('boscoe')->setValue(true);

    $input = new Input();
    $input->setImage('https://samples.clarifai.com/puppy.jpeg')->isUrl()
        ->setConcepts([$concept]);

    $inputResult = $clarifai->getInputRepository()->add($input);
```

#### Add input with metadata

In addition to adding an input with concepts, you can also add an input with
custom metadata. This metadata will then be searchable. Metadata can be any
arbitrary JSON.

```php
    $input = new Input();
    $input->setImage('https://samples.clarifai.com/metro-north.jpg')->isUrl()
        ->setMetaData([['key' => 'value', 'list' => [1, 2, 3]]);
    $inputResult = $clarifai->getInputRepository()->add($input);
```

#### Add input with a crop

When adding an input, you can specify crop points. The API will crop the image
and use the resulting image. Crop points are given as percentages from the top
left point in the order of top, left, bottom and right.

As an example, if you provide a crop as `0.2, 0.4, 0.3, 0.6` that means the
cropped image will have a top edge that starts 20% down from the original top
edge, a left edge that starts 40% from the original left edge, a bottom edge
that starts 30% from the original top edge and a right edge that starts 60% from
the original left edge.

```php
    $input = new Input();
    $input->setImage('https://samples.clarifai.com/metro-north.jpg')->isUrl()
        ->setCrop([0.2, 0.4, 0.3, 0.6]);
    $inputResult = $clarifai->getInputRepository()->add($input);
```

#### Get Inputs

You can list all the inputs (images) you have previously added either for search
or train.

If you added inputs with concepts, they will be returned in the response as well.

```php
    $inputResult = $clarifai->getInputRepository()->get();
```

#### Get Input by Id

If you'd like to get a specific input by id, you can do that as well.

```php
    $inputResult = $clarifai->getInputRepository()->getById('id');
```

#### Get Inputs Status

If you add inputs in bulk, they will process in the background. You can get the
status of all your inputs (processed, to_process and errors) like this:

```php
    $inputResult = $clarifai->getInputRepository()->getStatus();
```

#### Update input with concepts

To update an input with a new concept, or to change a concept value from true/false, you can do that:

```php
    $concept1 = new Concept();
    $concept1->setId('tree')->setValue(true);
    
    $concept2 = new Concept();
    $concept2->setId('water')->setValue(false);

    $modelResult = $clarifai->getInputRepository()->mergeInputConcepts([$inputId => [$concept1, $concept2]]);
```

#### Delete concepts from input

To remove concepts that were already added to an input, you can do this:

```php
    $concept1 = new Concept();
    $concept1->setId('mattid2')->setValue(true);
    
    $concept2 = new Concept();
    $concept2->setId('ferrari')->setValue(false);

    $modelResult = $clarifai->getInputRepository()->deleteInputConcepts([$inputId => [$concept1, $concept2]]);
```

#### Bulk update inputs with concepts

You can update an existing input using its Id. This is useful if you'd like to add concepts to an input after its already been added.

```php
    $concept1 = new Concept();
    $concept1->setId('tree')->setValue(true);
    
    $concept2 = new Concept();
    $concept2->setId('water')->setValue(false);
    
    $concept3 = new Concept();
    $concept3->setId('mattid2')->setValue(true);
    
    $concept4 = new Concept();
    $concept4->setId('ferrari')->setValue(false);

    $modelResult = $clarifai->getInputRepository()->mergeInputConcepts(
        [
            $inputId1 => [$concept1, $concept2],
            $inputId2 => [$concept3, $concept4],
        ]
    );
```

#### Bulk delete concepts from list of inputs

You can bulk delete multiple concepts from a list of inputs:

```php
    $concept1 = new Concept();
    $concept1->setId('tree')->setValue(true);
    
    $concept2 = new Concept();
    $concept2->setId('water')->setValue(false);
    
    $concept3 = new Concept();
    $concept3->setId('mattid2')->setValue(true);
    
    $concept4 = new Concept();
    $concept4->setId('ferrari')->setValue(false);

    $modelResult = $clarifai->getInputRepository()->deleteInputConcepts(
        [
            $inputId1 => [$concept1, $concept2],
            $inputId2 => [$concept3, $concept4],
        ]
    );
```

#### Delete Input By Id

You can delete a single input by id

```php
    $inputResult = $clarifai->getInputRepository()->deleteById('id');
```

#### Delete A List Of Inputs

You can also delete multiple inputs in one API call. This will happen
asynchronously.

```php
    $inputResult = $clarifai->getInputRepository()->deleteByIdArray(['id1', 'id2']);
```

#### Delete All Inputs

If you would like to delete all inputs from an application, you can do that as
well. This will happen asynchronously.

```php
    $inputResult = $clarifai->getInputRepository()->deleteAll();
```

## Models

There are many methods to work with models.

#### Create Model

You can create your own model and train it with your own images and concepts. Once you train it to see how you would like it to see, you can then use that model to make predictions.

When you create a model you give it a name and an id. If you don't supply an id, one will be created for you. All models must have unique ids.

```php
    $model = new Model();
    $model->setId('petsID');
    $modelResult = $clarifai->getModelRepository()->create($model);
```

#### Create Model with Concepts

You can also create a model and initialize it with the concepts it will contain. You can always add and remove concepts later.

```php
    $concept = new Concept();
    $concept->setId('boscoe');

    $model= new Model();
    $model->setId('petsId')
        ->setConcepts([$concept])
        ->setConceptsMutuallyExclusive(false)
        ->setClosedEnvironment(false);

    $modelResult = $clarifai->getModelRepository()->create($model);
```

#### Add Concepts To A Model

You can add concepts to a model at any point. As you add concepts to inputs, you may want to add them to your model.

```php
    $concept = new Concept();
    $concept->setId('dogs');

    $modelResult = $clarifai->getModelRepository()->mergeModelConcepts([$modelId => [$concept]]);
```

#### Remove Concepts From A Model

Conversely, if you'd like to remove concepts from a model, you can also do that.

```php
    $concept = new Concept();
    $concept->setId('dogs');

    $modelResult = $clarifai->getModelRepository()->deleteModelConcepts([$modelId => [$concept]]);
```

#### Update Model Name and Configuration

Here we will change the model name to 'newname' and the model's configuration to have concepts_mutually_exclusive=true and closed_environment=true.

```php
    $model->setName('newname')
        ->setClosedEnvironment(true)
        ->setConceptsMutuallyExclusive(true);

    $modelResult = $clarifai->getModelRepository()->update($model);
```

#### Get Models

To get a list of all models including models you've created as well as public models

```php
    $modelResult = $clarifai->getModelRepository()->get();
```

#### Get Model By Id

All models have unique Ids. You can get a specific model by its id:

```php
    $modelResult = $clarifai->getModelRepository()->getById($modelId);
```

#### Get Model Output Info By Id

The output info of a model lists what concepts it contains.

```php
    $modelResult = $clarifai->getModelRepository()->getOutputInfoById($modelId);
```

#### List Model Versions

Every time you train a model, it creates a new version. You can list all the versions created.

```php
    $modelResult = $clarifai->getModelRepository()->getModelVersions($modelId);
```

#### Get Model Version By Id

To get a specific model version, you must provide the modelId as well as the versionId. You can inspect the model version status to determine if your model is trained or still training.

```php
    $modelResult = $clarifai->getModelRepository()->getModelVersionById($modelId, $versionId);
```

#### Get Model Training Inputs

You can list all the inputs that were used to train the model.

```php
    $modelResult = $clarifai->getModelRepository()->getTrainingInputsById($modelId);
```

#### Get Model Training Inputs By Version

You can also list all the inputs that were used to train a specific model version.

```php
    $modelResult = $clarifai->getModelRepository()->getTrainingInputsByVersion($modelId, $versionId);
```

#### Delete A Model

You can delete a model using the modelId.

```php
    $modelResult = $clarifai->getModelRepository()->deleteById($modelId);
```

#### Delete A Model Version

You can also delete a specific version of a model with the modelId and versionId.

```php
    $modelResult = $clarifai->getModelRepository()->deleteVersionById($modelId, $versionId);
```

#### Delete All Models

If you would like to delete all models associated with an application, you can also do that. Please proceed with caution as these cannot be recovered.

```php
    $modelResult = $clarifai->getModelRepository()->deleteAll();
```

#### Train A Model

When you train a model, you are telling the system to look at all the images with concepts you've provided and learn from them. This train operation is asynchronous. It may take a few seconds for your model to be fully trained and ready.

Note: you can repeat this operation as often as you like. By adding more images with concepts and training, you can get the model to predict exactly how you want it to.

```php
    $modelResult = $clarifai->getModelRepository()->train($id);
```

#### Search Models By Name And Type

You can search all your models by name and type of model.

```php
    $modelResult = $clarifai->getSearchModelRepository()->searchByNameAndType($modelName, 'concept');
```

## Searches

#### Search By Predicted Concepts

When you add an input, it automatically gets predictions from the general model. You can search for those predictions.

```php
    $concept1 = new Concept();
    $concept1->setName('dog')->setValue(true);
    
    $concept2 = new Concept();
    $concept2->setName('cat');
        
    $inputResult = $clarifai->getSearchInputRepository()->searchByPredictedConcepts([$concept1, $concept2]);
```

#### Search By User Supplied Concept

After you have added inputs with concepts, you can search by those concepts.

```php
    $concept1 = new Concept();
    $concept1->setName('dog');
    
    $concept2 = new Concept();
    $concept2->setName('cat');
        
    $inputResult = $clarifai->getSearchInputRepository()->searchByUserSuppliedConcepts([$concept1, $concept2]);
```

#### Search By Custom Metadata

After you have added inputs with custom metadata, you can search by that metadata.

```php
    $metadata = ['key'=> 'value'];

    $inputResult = $clarifai->getSearchInputRepository()->searchByCustomMetadata([$metadata]);
```

#### Search By Reverse Image

You can use images to do reverse image search on your collection. The API will return ranked results based on how similar the results are to the image you provided in your query.

```php
    $input = new Input();
    $input->setImage('https://samples.clarifai.com/metro-north.jpg');
        
    $inputResult = $clarifai->getSearchInputRepository()->searchByReversedImage([$input]);
```

#### Search Match Url

You can also search for an input by URL.

```php
    $input = new Input();
    $input->setImage('https://samples.clarifai.com/metro-north.jpg');
        
    $inputResult = $clarifai->getSearchInputRepository()->searchByMatchUrl([$input]);
```

#### Search By Concept And Predictions

You can combine a search to find inputs that have concepts you have supplied as well as predictions from your model.

```php
    $concept1 = new Concept();
    $concept1->setName('dog');
    
    $concept2 = new Concept();
    $concept2->setName('cat');
        
    $inputResult = $clarifai->getSearchInputRepository()->search(
        [
            \DarrynTen\Clarifai\Repository\SearchInputRepository::INPUT_CONCEPTS => [$concept1],
            \DarrynTen\Clarifai\Repository\SearchInputRepository::OUTPUT_CONCEPTS => [$concept2]
        ]
    );
```

#### Search ANDing

You can also combine searches using AND.

```php
    $concept1 = new Concept();
    $concept1->setName('dog');
    
    $concept2 = new Concept();
    $concept2->setName('cat');
    
    $input = new Input();
    $input->setImage('https://samples.clarifai.com/metro-north.jpg');
    
    $metadata = ['key' => 'value'];
        
    $inputResult = $clarifai->getSearchInputRepository()->search(
        [
            SearchInputRepository::INPUT_CONCEPTS => [$concept1],
            SearchInputRepository::OUTPUT_CONCEPTS => [$concept2],
            SearchInputRepository::IMAGE => [$input],
            SearchInputRepository::METADATA => [$metadata],
        ]
    );
```

## Pagination

Many API calls are paginated. You can provide page and per_page params to the API. In the example below we are getting all inputs and specifying to start at page 2 and get back 20 results per page.

```php
    $inputResult = $clarifai->getInputRepository()->setPage(2)->setPerPage(20)->get();
```

## Patching(Only for Concepts)

We designed PATCH to work over multiple resources at the same time (bulk) and be flexible enough for all your needs to minimize round trips to the server. Therefore it might seem a little different to any PATCH you've seen before, but it's not complicated. All three actions that are supported do overwrite by default, but have special behaviour for lists of objects (for example lists of concepts).

#### Merge

Merge action will overwrite a key:value with key:new_value or append to an existing list of values, merging dictionaries that match by a corresponding id field.

In the following examples A is being patched into B to create the Result:

```
  *Merges different key:values*
  A = `{"a":[1,2,3]}`
  B = `{"blah":true}`
  Result = `{"blah":true, "a":[1,2,3]}`
  
  *For id lists, merge will append*
  A = `{"a":[{"id": 1}]}`
  B = `{"a":[{"id": 2}]}`
  Result = `{"a":[{"id": 2}, {"id":1}]}`
  
  *Simple merge of key:values and within a list*
  A = `{"a":[{"id": "1", "other":true}], "blah":1}`
  B = `{"a":[{"id": "2"},{"id":"1", "other":false}]}`
  Result = `{"a":[{"id": "2"},{"id": "1"}], "blah":1}`
  
  *Different types should overwrite fine*
  A = `{"a":[{"id": "1"}], "blah":1}`
  B = `{"a":[{"id": "2"}], "blah":"string"}`
  Result = `{"a":[{"id": "2"},{"id": "1"}], "blah":1}`
  
  *Deep merge, notice the "id":"1" matches, so those dicts are merged in the list*
  A = `{"a":[{"id": "1","hey":true}], "blah":1}`
  B = `{"a":[{"id": "1","foo":"bar","hey":false},{"id":"2"}], "blah":"string"}`
  Result = `{"a":[{"hey":true,"id": "1","foo":"bar"},{"id":"2"}], "blah":1}`
  
  *For non-id lists, merge will append*
  A = `{"a":[{"blah": "1"}], "blah":1}`
  B = `{"a":[{"blah": "2"}], "blah":"string"}`
  Result = `{"a":[{"blah": "2"}, {"blah":"1"}], "blah":1}`
  
  *For non-id lists, merge will append*
  A = `{"a":[{"blah": "1"}], "blah":1, "dict":{"a":1,"b":2}}`
  B = `{"a":[{"blah": "2"}], "blah":"string"}`
  Result = `{"a":[{"blah": "2"}, {"blah":"1"}], "blah":1, "dict":{"a":1,"b":2}}`
  
  *Simple overwrite root element*
  A = `{"key1":true}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":true}`
  
  *Overwrite a sub element*
  A = `{"key1":{"key2":true}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":true, "key3":"value3"}}`
  
  *Merge a sub element*
  A = `{"key1":{"key2":{"key4":"value4"}}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":{"key4":"value4"}, "key3":"value3"}}`
  
  *Merge multiple trees*
  A = `{"key1":{"key2":{"key9":"value9"}, "key3":{"key4":"value4", "key10":[1,2,3]}}, "key6":{"key11":"value11"}}`
  B = `{"key1":{"key2":"value2", "key3":{"key4":{"key5":"value5"}}}, "key6":{"key7":{"key8":"value8"}}}`
  Result = `{"key1":{"key2":{"key9":"value9"}, "key3":{"key4":"value4", "key10":[1,2,3]}}, "key6":{"key7":{"key8":"value8"}, "key11":"value11"}}`
  
  *Merge {} element will replace*
  A = `{"key1":{"key2":{}}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":{}, "key3":"value3"}}`
  
  *Merge a null element does nothing*
  A = `{"key1":{"key2":null}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":"value2", "key3":"value3"}}`
  
  *Merge a blank list [] will replace root element*
  A = `{"key1":[]}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":[]}`
  
  *Merge a blank list [] will replace single element*
  A = `{"key1":{"key2":[]}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":[], "key3":"value3"}}`
  
  *Merge a blank list [] will remove nested objects*
  A = `{"key1":{"key2":[{"key3":"value3"}]}}`
  B = `{"key1":{"key2":{"key3":"value3"}}}`
  Result = `{"key1":{"key2":[{"key3":"value3"}]}}`
  
  *Merge an existing list with some other struct*
  A = `{"key1":{"key2":{"key3":[{"key4":"value4"}]}}}`
  B = `{"key1":{"key2":[]}}`
  Result = `{"key1":{"key2":{"key3":[{"key4":"value4"}]}}}`
```

#### Remove

Remove action will overwrite a key:value with key:new_value or delete anything in a list that matches the provided values' ids.

In the following examples A is being patched into B to create the Result:
```
  *Remove from list*
  A = `{"a":[{"id": "1"}], "blah":1}`
  B = `{"a":[{"id": "2"},{"id": "3"}, {"id":"1"}], "blah":"string"}`
  Result = `{"a":[{"id": "2"},{"id":"3"}], "blah":1}`
  
  *For non-id lists, remove will append*
  A = `{"a":[{"blah": "1"}], "blah":1}`
  B = `{"a":[{"blah": "2"}], "blah":"string"}`
  Result = `{"a":[{"blah": "2"}, {"blah":"1"}], "blah":1}`
  
  *Empty out a nested dictionary*
  A = `{"key1":{"key2":true}}`
  B = `{"key1":{"key2":"value2"}}`
  Result = `{"key1":{}}`
  
  *Remove the root element, should be empty*
  A = `{"key1":true}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{}`
  
  *Remove a sub element*
  A = `{"key1":{"key2":true}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key3":"value3"}}`
  
  *Remove a multiple sub elements*
  A = `{"key1":{"key2":{"key3":true}, "key4":true}}`
  B = `{"key1":{"key2":{"key3":{"key5":"value5"}}, "key4":{"key6":{"key7":"value7"}}}}`
  Result = `{"key1":{"key2":{}}}`
  
  *Remove one of the root elements if there are more than one*
  A = `{"key1":true}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}, "key4":["a", "b", "c"]}`
  Result = `{"key4":["a", "b", "c"]}`
  
  *Remove with false should over write*
  A = `{"key1":{"key2":false, "key3":true}, "key4":false}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}, "key4":[{"key5":"value5", "key6":"value6"}, {"key7": "value7"}]}`
  Result = `{"key1":{"key2":false}, "key4":false}`
  
  *Only objects with id's can be put into lists*
  A = `{"key1":[{"key2":true}]}`
  B = `{"key1":[{"key2":"value2"}, {"key3":"value3"}]}`
  Result = `{}`
  
  *Elements with {} should do nothing*
  A = `{"key1":{}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":"value2", "key3":"value3"}}`
  
  *Elements with nil should do nothing*
  A = `{"key1":{"key2":null}}`
  B = `{"key1":{"key2":"value2", "key3":"value3"}}`
  Result = `{"key1":{"key2":"value2", "key3":"value3"}}`
```

#### Overwrite

Overwrite action will overwrite a key:value with key:new_value or overwrite a list of values with the new list of values. In most cases this is similar to merge action.

In the following examples A is being patched into B to create the Result:

```
  *Overwrite whole list*
  A = `{"a":[{"id": "1"}], "blah":1}`
  B = `{"a":[{"id": "2"}], "blah":"string"}`
  Result = `{"a":[{"id": "1"}], "blah":1}`
  
  *For non-id lists, overwrite will overwrite whole list*
  A = `{"a":[{"blah": "1"}], "blah":1}`
  B = `{"a":[{"blah": "2"}], "blah":"string"}`
  Result = `{"a":[{"blah": "1"}], "blah":1}`
```

# Roadmap

- [x] Train
  - [x] Add Image with Concepts
  - [x] Create a Model
  - [x] Train a Model
  - [x] Predict with a Model
- [ ] Search
  - [ ] Add Image to Search
  - [ ] Search by Concept
  - [ ] Reverse Image Search
- [ ] Applications
- [ ] Languages

## Public Model IDs

- General - aaa03c23b3724a16a56b629203edc62c
- Food - bd367be194cf45149e75f01d59f77ba7
- Travel - eee28c313d69466f836ab83287a54ed9
- NSFW - e9576d86d2004ed1a38ba0cf39ecb4b1
- Weddings - c386b7a870114f4a87477c0824499348
- Colour - eeed0b6733a644cea07cf4c60f87ebb7
- Face Detection - a403429f2ddf4b49b307e318f00e528b
- Apparel - e0be3b9d6a454f0493ac3a30784001ff
- Celebrity - e466caa0619f444ab97497640cefc4dc

## Supported Images

* JPEG
* PNG
* TIFF
* BMP

## Caching

Because these are expensive calls (time and money) some of them can
benefit from being cached. All caching should be off by default and only
used if explicity set.

These run through the `darrynten/any-cache` package, and no extra config
is needed. Please ensure that any features that include caching have it
be optional and initially set to `false` to avoid unexpected behaviour.

## Contributing and Testing

There is currently 100% test coverage in the project, please ensure that
when contributing you update the tests. For more info see CONTRIBUTING.md

We would love help getting decent documentation going, please get in touch
if you have any ideas.

## Acknowledgements

* [Dmitry Semenov](https://github.com/mxnr) for jumping on board.
* [Andrei Voitik](https://github.com/vojtik) for all his great input.
