<!-- loiob6b6c24abe0b442eb69143d83e45a6c9 -->

# Prior Labs

Tabular foundation models from Prior Labs generate predictions from business data containing numerical, categorical, and textual information without requiring extensive data preparation or traditional model training. You can use these models to solve classification and regression problems across business functions.

The generative AI hub in SAP AI Core provides access to the TabPFN-3.5 family of tabular foundation models from Prior Labs, part of SAP. The family currently includes TabPFN-3.5 Plus, a model designed to solve prediction problems using tabular data across industries and business functions. This model helps you to incorporate accurate predictions into business processes while reducing the effort required for data preparation and model training.

Traditional machine learning approaches often assume that rows are independent, features are clean and well structured, and sufficient data is available for training. Business data, however, rarely meets all these assumptions. For example, an invoice can combine monetary values with customer identifiers, while a purchase order can contain supplier information, material descriptions, and requested delivery dates. This combination of numerical, categorical, textual, and other business data can provide valuable signals for predicting outcomes such as payment delays, forecasting cash flow, and supporting operational decisions.

Extracting these signals with conventional machine learning can require significant data preparation, feature engineering, and model training. TabPFN-3.5 is designed to reduce this effort, particularly for challenging tabular datasets. It can work with text-rich tables, categorical features with many distinct values, and datasets containing large numbers of input features, without requiring extensive cleaning or transformation before the data can be used for prediction.



## Business Use Cases

The TabPFN-3.5 family of models supports a wide range of prediction scenarios across business functions. These include:

-   **Demand and inventory planning:** Estimate product demand to support purchasing decisions and optimize supply chain operations.
-   **Finance and cash-flow management:** Predict which invoices are likely to be paid late to improve cash-flow forecasting and planning.
-   **Supplier reliability:** Predict delays in the delivery of purchase orders to identify potential risks and mitigate production disruptions.
-   **Production and quality management:** Identify production batches that are at risk of failing quality control checks.
-   **Workforce planning:** Estimate the time required to complete maintenance activities and improve scheduling efficiency.
-   **Sales forecasting:** Predict which prospects are most likely to convert and prioritize outreach activities.
-   **Customer retention:** Identify customers at risk of churn and proactively engage them through support and retention programs.
-   **Spend classification:** Predict purchasing categories from line-item data to improve spend analysis and reporting.



## Prediction Tasks and Request Formats

The `/predict` endpoint supports classification and regression prediction tasks.

-   Classification: Predicts a category value. A maximum of 160 classes is supported.

-   Regression: Predicts a numeric value.


The `/predict` endpoint accepts the following request formats:

-   `application/json`: Includes datasets directly in the JSON request body.

-   `multipart/form-data`: Uploads datasets as `CSV` or Apache Parquet files. A `request` text field contains the request configuration.




## Request Fields

The prediction request contains the following required fields.


<table>
<tr>
<th valign="top">

**Field**

</th>
<th valign="top">

**Required**

</th>
<th valign="top">

**Description**

</th>
</tr>
<tr>
<td valign="top">

`x_train`

</td>
<td valign="top">

Yes

</td>
<td valign="top">

Training features. One row per labeled example.

</td>
</tr>
<tr>
<td valign="top">

`y_train`

</td>
<td valign="top">

Yes

</td>
<td valign="top">

Training target. Must contain exactly one column aligned row-for-row with `x_train`.

</td>
</tr>
<tr>
<td valign="top">

`x_test`

</td>
<td valign="top">

Yes

</td>
<td valign="top">

Rows for prediction. Must contain the same columns as `x_train`.

</td>
</tr>
<tr>
<td valign="top">

`task_config`

</td>
<td valign="top">

Yes

</td>
<td valign="top">

Task type and model configuration.

</td>
</tr>
</table>



## Task Configuration Fields


<table>
<tr>
<th valign="top">

**Field**

</th>
<th valign="top">

**Default**

</th>
<th valign="top">

**Description**

</th>
</tr>
<tr>
<td valign="top">

`task_config.task`

</td>
<td valign="top">

Required

</td>
<td valign="top">

Prediction task type. Supported values are `classification` and `regression`.

</td>
</tr>
<tr>
<td valign="top">

`task_config.tabpfn_config`

</td>
<td valign="top">

\{\}

</td>
<td valign="top">

Model configuration settings.

</td>
</tr>
<tr>
<td valign="top">

`task_config.predict_params`

</td>
<td valign="top">

\{\}

</td>
<td valign="top">

Prediction output settings.

</td>
</tr>
</table>



## Model Configuration Options

The `tabpfn_config` object provides optional settings for model inference.


<table>
<tr>
<th valign="top">

**Configuration**

</th>
<th valign="top">

**Type**

</th>
<th valign="top">

**Default**

</th>
<th valign="top">

**Description**

</th>
</tr>
<tr>
<td valign="top">

`n_estimators`

</td>
<td valign="top">

Integer

</td>
<td valign="top">

8

</td>
<td valign="top">

Number of ensemble members used for prediction

</td>
</tr>
<tr>
<td valign="top">

`softmax_temperature`

</td>
<td valign="top">

Float

</td>
<td valign="top">

Model default

</td>
<td valign="top">

Controls the confidence of predicted probability distributions

</td>
</tr>
<tr>
<td valign="top">

`average_before_softmax`

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

Model default

</td>
<td valign="top">

Specifies whether ensemble outputs are averaged before or after softmax

</td>
</tr>
<tr>
<td valign="top">

`balance_probabilities`

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

Model default

</td>
<td valign="top">

Rebalances class probabilities for classification tasks

</td>
</tr>
<tr>
<td valign="top">

`categorical_features_indices`

</td>
<td valign="top">

List of integers

</td>
<td valign="top">

Auto-detected

</td>
<td valign="top">

Defines columns that are to be treated as categorical features

</td>
</tr>
<tr>
<td valign="top">

`random_state`

</td>
<td valign="top">

Integer

</td>
<td valign="top">

Unset

</td>
<td valign="top">

Seed value for reproducible predictions

</td>
</tr>
<tr>
<td valign="top">

`inference_precision`

</td>
<td valign="top">

String

</td>
<td valign="top">

Model default

</td>
<td valign="top">

Numeric precision used during inference

</td>
</tr>
<tr>
<td valign="top">

`fit_mode`

</td>
<td valign="top">

Enum

</td>
<td valign="top">

fit\_preprocessors

</td>
<td valign="top">

Preprocessing and caching strategy

</td>
</tr>
<tr>
<td valign="top">

`memory_saving_mode`

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

Automatic

</td>
<td valign="top">

Reduces memory consumption for large datasets

</td>
</tr>
<tr>
<td valign="top">

`inference_config`

</td>
<td valign="top">

Object

</td>
<td valign="top">

Unset

</td>
<td valign="top">

Advanced inference settings

</td>
</tr>
</table>



## Classification Output Options


<table>
<tr>
<th valign="top">

**Output Type**

</th>
<th valign="top">

**Returns**

</th>
</tr>
<tr>
<td valign="top">

`probas`

</td>
<td valign="top">

Per-class probabilities for each test row

</td>
</tr>
<tr>
<td valign="top">

`preds`

</td>
<td valign="top">

Predicted class label for each test row

</td>
</tr>
<tr>
<td valign="top">

`top_k`

</td>
<td valign="top">

Most probable classes ranked by likelihood; requires the `top_k` parameter

</td>
</tr>
</table>



## Regression Output Options


<table>
<tr>
<th valign="top">

**Output Type**

</th>
<th valign="top">

**Returns**

</th>
</tr>
<tr>
<td valign="top">

`mean`

</td>
<td valign="top">

Predicted mean value

</td>
</tr>
<tr>
<td valign="top">

`median`

</td>
<td valign="top">

Predicted median value

</td>
</tr>
<tr>
<td valign="top">

`mode`

</td>
<td valign="top">

Predicted mode value

</td>
</tr>
<tr>
<td valign="top">

`quantiles`

</td>
<td valign="top">

Values for requested quantiles

</td>
</tr>
<tr>
<td valign="top">

`main`

</td>
<td valign="top">

Point estimates and quantile values

</td>
</tr>
<tr>
<td valign="top">

`full`

</td>
<td valign="top">

Prediction statistics and predictive distributions

</td>
</tr>
</table>



### Additional Parameters

-   `quantiles`: List of values between 0 and 1 used with `quantiles` output
-   `top_k`: Number of classes returned when using `top_k`



## Response Fields


<table>
<tr>
<th valign="top">

**Field**

</th>
<th valign="top">

**Description**

</th>
</tr>
<tr>
<td valign="top">

`prediction`

</td>
<td valign="top">

Prediction results

</td>
</tr>
<tr>
<td valign="top">

`class_labels`

</td>
<td valign="top">

Sorted list of class labels observed during training \(available only for classification tasks\)

</td>
</tr>
<tr>
<td valign="top">

`metadata`

</td>
<td valign="top">

Execution details

</td>
</tr>
<tr>
<td valign="top">

`usage`

</td>
<td valign="top">

Dataset statistics and ensemble information

</td>
</tr>
<tr>
<td valign="top">

`model_id`

</td>
<td valign="top">

Model identifier, if available

</td>
</tr>
</table>



## Prediction Result Formats


<table>
<tr>
<th valign="top">

**Task and Output Type**

</th>
<th valign="top">

**Shape**

</th>
</tr>
<tr>
<td valign="top">

Classification / `probas`

</td>
<td valign="top">

List of class probability values for each prediction row

</td>
</tr>
<tr>
<td valign="top">

Classification / `preds`

</td>
<td valign="top">

List of predicted labels

</td>
</tr>
<tr>
<td valign="top">

Regression / `mean`, `median`, `mode`

</td>
<td valign="top">

List of numeric values

</td>
</tr>
<tr>
<td valign="top">

Regression / `quantiles`

</td>
<td valign="top">

List of quantile values

</td>
</tr>
<tr>
<td valign="top">

Regression / `full`

</td>
<td valign="top">

Object containing prediction statistics and distributions

</td>
</tr>
</table>

> ### Note:  
> Non-finite values, such as positive or negative infinity, are serialized as `null`.



## Metadata Fields


<table>
<tr>
<th valign="top">

**Field**

</th>
<th valign="top">

**Description**

</th>
</tr>
<tr>
<td valign="top">

`task`

</td>
<td valign="top">

Processed prediction task

</td>
</tr>
<tr>
<td valign="top">

`test_set_num_rows`

</td>
<td valign="top">

Number of rows submitted for prediction

</td>
</tr>
<tr>
<td valign="top">

`test_set_num_cols`

</td>
<td valign="top">

Number of feature columns after preprocessing

</td>
</tr>
<tr>
<td valign="top">

`tabpfn_config`

</td>
<td valign="top">

Resolved model configuration, including default values applied during execution

</td>
</tr>
</table>

-   **[TabPFN-3.5 Plus](tabpfn-3-5-plus-10eee99.md "Deploy the TabPFN-3.5 Plus model and use example requests to perform classification and
    regression tasks on tabular data.")**  
Deploy the TabPFN-3.5 Plus model and use example requests to perform classification and regression tasks on tabular data.

