# Week 2 - Statistical Bias and Feature Importance

## 1. Statistical Bias

* Training data does not comprehensively represent the problem space.
* Some elements of a dataset are more heavily weighted or represented.
* Biased datasets lead to biased models
    * Imbalances in product review dataset

## 2. Statistical Bias Causes

* Activity Bias
    * Social media content
    * Data on social media represents only a small %age of population

* Societal Bias
    * Human Generated content
    * Not on social media
    * Unconscious bias

* Selection Bias
    * Feedback loop
    * Limited choices given to a user (for survey) can bias the data

* Data Drift
    * Covariant Drift - distribution of independent variables or **features** can change
    * Prior Probability Drift - distribution of **labels** or targeted variables might change
    * Concept Drift - relationship between **features** and **labels** can change

## 3. Measuring Statistical Bias

* Class Imbalance (CI)
    * Measures the imbalance in the number of members between different facet values
    * Example: Does a product_category has disproportionately **more reviews** than others?

* Difference in Proportions of Labels (DPL)
    * Measures the imbalance of +ve outcomes between different facet values.
    * Example: Does a product_category has disproportionately **higher ratings** than others?

## 4. Detecting Statistical Bias

* Tools to detect statistical bias:
    1. SageMaker Data Wrangler
        * Provides with capabilities to connect to various different sources for data.
        * Visualize the data and transform the data by applying any number of transformations in the Data Wrangler environment.
        * Detect statistical bias in data sets and generate reports about the bias detected in those data sets.
        * It also provides capabilities to provide feature importance calculations on training data set.
    2. SageMaker Clarify

        ```py
        """
        Clarify Processor
        """

        from sagemaker import clarify

        clarify_processir = clarify.SageMakerClarifyProcessor(
            role=role,

            # Distributed cluster size
            instance_count=1,

            # Type of instance
            instance_type="ml.c5.2xlarge",
            sagemaker_session=sess
        )
        # S3 location to store the bias report
        bias_report_output_path = ""
        ```

        ```py
        """
        Data Configuration
        """

        bias_data_config = clarify.DataConfig(
            s3_data_input_path="",
            s3_output_path="",
            label="sentiment"
            headers=df_balanced.coloumns.to_list(),
            dataset_type="text/csv"
        )
        ```

        ```py
        """
        Bias Configuration
        """

        bias_config = clarify.BiasConfig(
            label_values_or_threashold=[...],
            facet_name="product_category"
        )
        ```

        ```py
        """
        Pre-Training Bias Job
        """

        clarify_processor.run_pre_training_bias(
            data_config=...,
            data_bias_config=...,
            methods=["", "", ...],
            wait=<<False/True>>
            logs=<<False/True>>
        )
        ```

## 5. Approaches to Statistical Bias Detection

* DataWrangler
    * GUI approach
* Clarify
    * API based approach
    * Scale-out possible

## 6. Feature Importance: SHAP

* Explains the individual features that make up the training data using a score called important score

* How useful or valuable the feature is relative to other features.

* Predict the sentiment for a product -> Which features play a role?

* SHAP - **SH**apley **A**dditive ex**P**lanations
    * Shapley values based on game theory
    * Explain predictions of a ML model
        * Each feature value of training data instance is a player in a game
        * ML prediction is the payout
    * Local vs global explanations
    * SHAP can guarantee consistency and local accuracy