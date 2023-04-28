# ⏳ ChatLog

![](./config/model_system_v3.png)

This repository stores data and code for the paper `ChatLog: Recording and Analysing ChatGPT Across Time` [[abs](https://arxiv.org/abs/2304.14106)][[pdf](https://arxiv.org/pdf/2304.14106.pdf)].

# Data

We release our data at [tsinghua cloud](https://cloud.tsinghua.edu.cn/d/733684efbec84cbb8c52/).

Now the category is as following:

- ChatLog-Monthly
  -  202303.zip
- ChatLog-Daily
  - api
    - everyday_20230305-20230409.zip
  - open
    - before0301.zip
  - processed_csv
    - avg_HC3_all_pearson_corr_feats.csv
    - avg_HC3_knowledge_pearson_corr_feats.csv

Every `zip` file contains some `jsonl` files and each json object is as the format:

| column name:  | id       | source_type                                      | source_dataset                    | source_task                                    | q                                                          | a                    | language         | chat_date                       | time                                               |
| ------------- | -------- | ------------------------------------------------ | --------------------------------- | ---------------------------------------------- | ---------------------------------------------------------- | -------------------- | ---------------- | ------------------------------- | -------------------------------------------------- |
| introduction: | id       | type of the source: from open-access dataset/api | dataset of the question come from | specific task name，such as sentiment analysis | question                                                   | response of  ChatGPT | language         | The time that ChatGPT responses | The time that the data is stored into our database |
| example       | 'id': 60 | 'source_type': 'open'                            | 'source_dataset': 'ChatTrans'     | 'source_task': 'translation'                   | 'q': 'translate this sentence into Chinese: Good morning', | 'a': '早上好',       | 'language': 'zh' | 'chat_date': '2023-03-03',      | 'time': '2023-03-04 09:58:09',                     |

The ChatLog-Monthly and ChatLog-Daily will be continuously updated.

# Analysis Code

1. For extracting all the knowledge and linguistic features, run:

```
sh process_new_data_v1.sh
```

2. For analyzing features and calculating variation, run:

```
sh analyse_var_and_classify_across_time_v1.sh
```

3. Use LightGBM that ensembles the features with RoBERTa to train a robust ChatGPT detector, run:

```
sh lgb_train_v2.sh
```

4. For trend and correlation analysis, first dumping knowledge features into `avg_HC3_knowledge_pearson_corr_feats.csv`

```
sh draw_knowledge_feats_v1.sh
```

5. Then dump other linguistic features into `avg_HC3_all_pearson_corr_feats.csv`

```
sh draw_eval_corr_v1.sh
```

6. Finally, we can draw heatmaps and lineplots for trend and correlation analysis:

   - Put the dumped  `avg_HC3_knowledge_pearson_corr_feats.csv` and  `avg_HC3_all_pearson_corr_feats.csv` under the `./shells` folder
   - Then use `./shells/knowledge_analysis.ipynb` and `./shells/temporal_analysis.ipynb` to draw every figure.

   
# Citation
If you find our work useful, please cite:

```
@article{tu2023chatlog,
  title={ChatLog: Recording and Analyzing ChatGPT Across Time},
  author={Tu, Shangqing and Li, Chunyang and Yu, Jifan and Wang, Xiaozhi and Hou, Lei and Li, Juanzi},
  journal={arXiv preprint arXiv:2304.14106},
  year={2023}
}
```