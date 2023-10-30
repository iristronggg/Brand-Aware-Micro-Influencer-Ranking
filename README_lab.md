# BAMIR 莊海因

Brand-Aware Micro-Influencer Ranking

## Data

由於資料集約 50GB，光碟中沒有。請到 NAS 及 Francia 取用。

influencer 部分是採用分卷壓縮，全部下載後請使用解壓縮軟體 (e.g. BetterZip) **一併**解壓縮，若單獨解壓縮會失敗。

### 文字

- dataset_text：貼文文字資料都在這邊。也包含帳號 biography 以及各種資料屬性。
- new_data：相關論文有用到的 engagement rate，以及用來計算 engagement rate 的「真實合作關係」貼文。

### 圖片

- brand：360 個品牌的圖片資料。
- influencer：網紅的圖片資料，第一層資料夾為品牌名稱，第二層資料夾才是該品牌合作的網紅名稱（也就是正確答案）。

要檢查可能會包含到 .DS_Store 資料，需要刪除。

## Code

### Concept

- 1_biography_to_concept.ipynb: 使用 ConceptNet 將 biography 中的字對到 concept，以及 profile business category。最後得到新的母集合 `train_concept_profile_id_dict_new.pkl`。在這邊也會產生每個帳號透過 1) bio, 2) profile business category 連到的 concept。
- 2_check_hashtag.ipynb: 從貼文中抓取出 #，得到帳號使用的 #，並統整博關注用的 #。
- 3_extract_related_hashtag.ipynb: 用統計網站蒐集 # 的相關 #，並且將其對應到 concept
- 4_account_hashtag_concept: 3) 使用 hashtag，加入各帳號與 concept 的關係，生成最後的 account-concept weighted pair，以及 concept-concept coocf hashtag 部份分數。
- 5_coocf_node2vec 計算 coocf mention 分數，跑 node2vec 得到 concept embedding，並 inference 最終得到 account concept repr。

—中間產物— 

- mor_cat_set: morning parent concept set
- train_concept_profile_id_dict_new.pkl
- high_tf_set: biography 中出現太多次的 token
- remain_concept_cat: 視覺化用，一些代表性的 concept 對應我自己標上的母類別。

### Text

- lda: LDA 的訓練與 inference。

### Image

- resnet: 使用 pre-trained ResNet 得到 image embedding。
- upernet: 使用 pre-trained UperNet 得到 image embedding。
    - 下載 https://github.com/CSAILVision/unifiedparsing.git ，請參考 requirements_upernet.txt 所需的套件與版本。版本有嚴格限制。

### Model

- model_bamir: 將前處理後的 embedding 排列成訓練所需，建立模型、訓練、測試。
    - 訓練模型所需的檔案都已經在 train_test_split 資料夾中。
    - Handle Input Data 部分就是為了處理跑模型所需檔案，可以不用跑。

### Other

- related_work_code: 三篇相關論文的程式碼，有些是在 github 上找到，並非附在論文中的。
    - https://dl.acm.org/doi/10.1145/3343031.3351080
    - https://ieeexplore.ieee.org/document/9454334
    - https://ieeexplore.ieee.org/document/9712372
- sorted_360_brand_list.pkl, sorted_3748_inf_list 為根據前幾篇論文篩選出他們使用的品牌、微網紅不重複名單，依照字母順序排列。brand account ID, inf account ID 都是根據這兩個清單。
    - 在訓練時另有 train, test node id，可以在 train_test_split 找到對應的 dictionary。
- category_list: 12 個品牌種類。category ID 是根據 list index。
- all_positive_tuples: 正合作關係的品牌與網紅，其帳號 ID 形成的 tuple。

# Recommendation

不太建議用同個資料集做同個任務，做起來感覺資料集本身已經到極限了。