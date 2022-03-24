Customize Configuration
==============================================
Here, we will show how to customize the extra parameters of your model in a configuration file.

We suppose the configuration file name is ``MeLU.yaml``

First, we configure dataset, training, valid and metrics parts. These parts are as same as RecBole.

.. code:: yaml

    # Dataset config
    USER_ID_FIELD: user_id
    ITEM_ID_FIELD: item_id
    RATING_FIELD: rating
    LABEL_FIELD: rating
    load_col:
        inter: [user_id, item_id, rating]
        item: [item_id,movie_title,release_year,class]
        user: [user_id,age,gender,occupation,zip_code]
    user_inter_num_interval: [13,100]

    # Training and valid config
    epochs: 2   # 30
    train_batch_size: 32
    valid_metric: 'NDCG@5'

    # Metrics
    metrics: ['NDCG']
    metric_decimal_place: 4
    topk: 5

Then, we will configure the evaluate part. We should choose ``task`` as the key of ``group_by``.

.. code:: yaml

    # Evaluate config
    eval_args:
        group_by: task
        order: RO
        split: {'RS': [0.8,0.1,0.1]}
        mode : labeled

Finally, we will configure the parameters of your model like this.

.. code:: yaml

    # Meta learning config
    meta_args:
        support_num: none
        query_num: 10

    # MeLU Parameters
    embedding_size: 32  # For 7 fields in the dataset.
    mlp_hidden_size: [224,64,64]
    melu_args:
        local_lr: 0.000005  # 5e-6
        lr: 0.00005 #5e-5

The complete code is as following.

.. code:: yaml

    ## MeLU.yaml ##

    # Dataset config
    USER_ID_FIELD: user_id
    ITEM_ID_FIELD: item_id
    RATING_FIELD: rating
    LABEL_FIELD: rating
    load_col:
        inter: [user_id, item_id, rating]
        item: [item_id,movie_title,release_year,class]
        user: [user_id,age,gender,occupation,zip_code]
    user_inter_num_interval: [13,100]

    # Training and evaluation config
    epochs: 2   # 30
    train_batch_size: 32
    valid_metric: 'NDCG@5'

    # Evaluate config
    eval_args:
        group_by: task
        order: RO
        split: {'RS': [0.8,0.1,0.1]}
        mode : labeled

    # Meta learning config
    meta_args:
        support_num: none
        query_num: 10

    # MeLU Parameters
    embedding_size: 32  # For 7 fields in the dataset.
    mlp_hidden_size: [224,64,64]
    melu_args:
        local_lr: 0.000005  # 5e-6
        lr: 0.00005 #5e-5

    # Metrics
    metrics: ['NDCG']
    metric_decimal_place: 4
    topk: 5
