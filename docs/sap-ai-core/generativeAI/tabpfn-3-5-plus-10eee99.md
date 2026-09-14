<!-- loio10eee99335a84f69b95d5227271f3406 -->

# TabPFN-3.5 Plus

Deploy the TabPFN-3.5 Plus model and use example requests to perform classification and regression tasks on tabular data.

The TabPFN-3.5 Plus model lets you solve prediction problems using tabular data across industries and business functions. The model helps you to incorporate accurate predictions into business processes while reducing the effort required for data preparation and model training.



## Prerequisites

Before using TabPFN-3.5 in the generative AI hub, complete the onboarding steps required for SAP AI Core.

-   You've completed the tasks described in [Initial Setup](https://help.sap.com/viewer/2d6c5984063c40a59eda62f4a9135bee/CLOUD/en-US/38c4599432d74c1d94e70f7c955a717d.html "Get started with SAP AI Core using the standard procedures for the SAP BTP, Cloud Foundry environment or Kyma environment.") :arrow_upper_right:.
-   You're using the **extended** service plan, which is required for the generative AI hub.



## Data Limits

TabPFN-3.5 Plus applies the following limits to prediction requests.


<table>
<tr>
<th valign="top">

**Limit Type**

</th>
<th valign="top">

**Supported Limit**

</th>
</tr>
<tr>
<td valign="top">

Input context rows \(with known outcomes\)

</td>
<td valign="top">

1,000,000

</td>
</tr>
<tr>
<td valign="top">

Input columns

</td>
<td valign="top">

2,000

</td>
</tr>
<tr>
<td valign="top">

Context and prediction table size

</td>
<td valign="top">

200 million cells

</td>
</tr>
<tr>
<td valign="top">

Classification categories

</td>
<td valign="top">

160

</td>
</tr>
</table>

Prediction requests support up to 1,000,000 prediction rows. Lower limits can apply depending on the size of the context dataset, the number of feature columns, and the size of the prediction output.

If a request contains 1,000,000 context rows, the maximum number of prediction rows is 250,000 before other limits are considered.

Responses are limited to 32 million output values. For example, when returning probabilities for all 160 classification categories, the maximum supported number of prediction rows is 200,000.

Full regression distribution outputs support a maximum of 400 prediction rows per request.

> ### Note:  
> The maximum number of supported prediction rows can vary based on the context dataset size, feature count, selected output type, and response size.



<a name="loio10eee99335a84f69b95d5227271f3406__deployment_information"/>

## Deployment Information

You can access foundation models by creating a deployment for the model that you want to use. To do so, you need an auth token from your SAP AI Core instance. For more information, see [Get an Auth Token](get-an-auth-token-0808d42.md) and [Create a Deployment](create-a-deployment-b32e7a8.md).

To create your deployment, you need the following information:

-   Scenario: `foundation-models`
-   `executableId`: `prior-labs`
-   Model name: `tabpfn-3.5-plus`
-   Model version: `2609`

> ### Note:  
> Make sure that the model is available in the generative AI hub before creating a deployment. For more information, see [Supported Models](https://github.tools.sap/I343697/generative-ai-hub-readme/blob/main/README.md#1-supported-models).

To use a specific version of a model or to upgrade model versions manually, specify the version of your model deployment. To upgrade automatically, specify the model version as `latest`. For more information, see [Model Lifecycle](model-lifecycle-313fe25.md). If a model version isn't listed, the model isn't applicable.

After you've created a deployment for your model, you can consume the model using prompts. To access the model, you need your deployment ID, which you can set as an environment variable.

Check that you've set the following headers:


<table>
<tr>
<th valign="top">

Header

</th>
<th valign="top">

Value

</th>
</tr>
<tr>
<td valign="top">

Authorization

</td>
<td valign="top">

Bearer $AUTH\_TOKEN

</td>
</tr>
<tr>
<td valign="top">

AI-Resource-Group

</td>
<td valign="top">

The resource group used in the activation steps

</td>
</tr>
<tr>
<td valign="top">

$DEPLOYMENT\_URL

</td>
<td valign="top">

The deployment URL for your generative AI model. For more information, see [Create a Deployment](create-a-deployment-b32e7a8.md).

Alternatively, you can replace the `$DEPLOYMENT_URL` placeholder in the curl command with your deployment URL.

</td>
</tr>
</table>



## Examples

The following examples show how to submit prediction requests by using the `/predict` endpoint.



## Example 1: Classification Request

The following example submits a classification request and returns the predicted class label.

```
curl --request POST \
  --url "$DEPLOYMENT_URL/predict" \
  --header 'AI-Resource-Group: default' \
  --header "Authorization: Bearer $AUTH_TOKEN" \
  --header 'Content-Type: application/json' \
  --data '{
  "task_config": {
    "task": "classification",
    "tabpfn_config": {
      "n_estimators": 8,
      "softmax_temperature": 0.9,
      "average_before_softmax": false,
      "balance_probabilities": true,
      "categorical_features_indices": [0, 2],
      "random_state": 0,
      "inference_precision": "auto",
      "fit_mode": "fit_preprocessors",
      "memory_saving_mode": false
    },
    "predict_params": {
      "output_type": "preds"
    }
  },
  "x_train": {
    "PRODUCT": ["Office Chair", "Server Rack"],
    "PRICE": [150.8, 2200],
    "ORDERDATE": ["10-1-2025", "12-1-2025"]
  },
  "y_train": {
    "COSTCENTER": ["Office Furniture", "Data Infrastructure"]
  },
  "x_test": {
    "PRODUCT": ["Couch"],
    "PRICE": [999.99],
    "ORDERDATE": ["10-2-2025"]
  }
}'
```



## Example 2: Regression Request

The following example submits a regression request and returns the predicted mean value.

```
curl --request POST \
  --url "$DEPLOYMENT_URL/predict" \
  --header 'AI-Resource-Group: default' \
  --header "Authorization: Bearer $AUTH_TOKEN" \
  --header 'Content-Type: application/json' \
  --data '{
  "task_config": {
    "task": "regression",
    "predict_params": {
      "output_type": "mean"
    }
  },
  "x_train": [
    [5.0, 2.0],
    [2.0, 1.5],
    [3.2, 0.7],
    [0.8, 2.8]
  ],
  "y_train": [1.0, 1.8, 1.6, 1.8],
  "x_test": [
    [1.5, 1.9],
    [2.4, 0.8],
    [3.6, 4.5]
  ]
}'
```



## Example 3: Regression Request with Advanced Configuration

The following example returns quantile predictions and configures additional inference options.

```
curl --request POST \
  --url "$DEPLOYMENT_URL/predict" \
  --header 'AI-Resource-Group: default' \
  --header "Authorization: Bearer $AUTH_TOKEN" \
  --header 'Content-Type: application/json' \
  --data '{
  "task_config": {
    "task": "regression",
    "predict_params": {
      "output_type": "quantiles",
      "quantiles": [0.1, 0.5, 0.9]
    },
    "tabpfn_config": {
      "n_estimators": 4,
      "random_state": 42,
      "memory_saving_mode": true
    }
  },
  "x_train": [
    [5.0, 2.0],
    [2.0, 1.5],
    [3.2, 0.7],
    [0.8, 2.8]
  ],
  "y_train": [1.0, 1.8, 1.6, 1.8],
  "x_test": [
    [1.5, 1.9],
    [2.4, 0.8]
  ]
}'
```



## Example 4: Multipart Request

The following example uploads training and prediction datasets as `CSV` files and provides the prediction configuration in the `request` field.

```
curl --request POST \
  --url "$DEPLOYMENT_URL/predict" \
  --header 'AI-Resource-Group: default' \
  --header "Authorization: Bearer $AUTH_TOKEN" \
  -F 'x_train=@x_train.csv' \
  -F 'y_train=@y_train.csv' \
  -F 'x_test=@x_test.csv' \
  -F 'request={"task_config":{"task":"classification","predict_params":{"output_type":"preds"}}}'
```



## Example 5: Classification Response

The following example shows a successful classification response returned by the `/predict` endpoint.

```
{
  "prediction": [[0.93, 0.05, 0.02], [0.08, 0.81, 0.11]],
  "class_labels": ["Data Infrastructure", "Office Furniture"],
  "metadata": {
    "task": "classification",
    "test_set_num_rows": 1,
    "test_set_num_cols": 11,
    "tabpfn_config": {
      "n_estimators": 8,
      "average_before_softmax": false,
      "balance_probabilities": true,
      "categorical_features_indices": [0, 2],
      "fit_mode": "fit_preprocessors",
      "inference_precision": "auto",
      "memory_saving_mode": false,
      "random_state": 0,
      "softmax_temperature": 0.9
    }
  },
  "usage": {
    "X_train": {
      "num_rows": 4,
      "num_cols": 3,
      "num_cells": 12
    },
    "y_train": {
      "num_rows": 4,
      "num_cols": 1,
      "num_cells": 4
    },
    "X_test": {
      "num_rows": 2,
      "num_cols": 3,
      "num_cells": 6
    },
    "num_cells": 22,
    "num_predictions": 2,
    "n_estimators": 8
  },
  "model_id": null
}
```

