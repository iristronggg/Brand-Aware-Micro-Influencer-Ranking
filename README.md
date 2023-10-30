# README

# Which Micro-influencers Are Better for Your Brand? A Multimodal Deep Learning Approach for Brand-aware Micro-influencer Ranking

In the age of the Internet, individuals are significantly influenced by the content creators (i.e., influencers) they follow on social media platforms, particularly young users who are adept at navigating social networks. Therefore, influencer marketing has emerged as a powerful replacement for traditional marketing and has become a crucial strategy for brands to execute their marketing campaigns. “Influencer marketing” denotes the collaboration between brands and influencers to promote products or services primarily through sponsored posts or other ways on social platforms.

Among influencers, micro-influencers are defined as those influencers with fewer followers but with high-level expertise on distinct topics. Due to their specialized knowledge, high engagement followers, high customer trust, and cost-effectiveness, an increasing number of brands are opting for micro-influencer marketing to effectively reach their target audiences.

For brands to achieve successful micro-influencer marketing, the selection of suitable influencers becomes a critical prerequisite. Previous studies predominantly concentrate on influencer detection for identifying influencers within a pool of social media users. Fewer studies consider individual brands and recommend micro-influencer rankings for each brand.

In this research, we propose a multimodal deep learning approach, **Brand-Aware Micro-Influencer Ranking (BAMIR)**, to predict influencer ranking for given brands based on their content on social media. We further incorporate previously unexploited features such as account interactions and hashtag information, along with employing a specific negative sampling strategy to enhance ranking performance. Experimental results provide empirical evidences of the effectiveness of our proposed method, outperforming the state-of-the-art methods across various evaluation metrics.

## Dataset

We utilize the brand-micro-influencer dataset proposed by [Gan et al., 2019](https://doi.org/10.1145/3343031.3351080). This dataset was collected from Instagram, which is widely recognized as the most popular social media platform for influencer marketing. The dataset contains 360 brands across 12 categories and 3,748 influencers. Moreover, we collect the profile business category of each account in the dataset.

There are 50 history posts for each account consisting of text, image, number of likes, number of comments, and timestamp. Also, the profile information of each account is presented with a biography and number of followers.

## Source Code

### Concept

- 1_biography_to_concept.ipynb: Map words in the biography to concepts and profile business categories via ConceptNet to obtain a new parent concept set. This step will also generate the concepts that each account is connected to through 1) biography and 2) profile business category.
- 2_check_hashtag.ipynb: Extract hashtags from the posts, gather hashtags used by each account, and eliminate hashtags solely for likes and follows to reduce noise.
- 3_extract_related_hashtag.ipynb: Collect related hashtags from a social media hashtag analysis [website](http://tagsfinder.com/), and associate them with concepts accordingly.
- 4_account_hashtag_concept: 3) Utilizing hashtags, establish the relationship between each account and concept to generate the final account-concept weighted pairs, as well as the concept-concept co-occurrence frequency score of hashtags.
- 5_coocf_node2vec: Calculate co-occurrence frequency score of mentioning. we employ node2vec (Grover & Leskovec, 2016) to train the concept node embeddings. Finally, account concept representation is inferenced.

### Text

- lda: LDA training and inference.

### Image

- resnet: Utilize pre-trained ResNet to obtain image object embedding.
- upernet: Utilize pre-trained UperNet to obtain image scene embedding.
    - Clone [UPerNet](https://github.com/CSAILVision/unifiedparsing.git). The package versions are referenced from requirements_upernet.txt.

### Model

- model_bamir: Organize the preprocessed embeddings. Build the model, train, and test.
