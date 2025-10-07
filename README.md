# Machine Learning Project Report - Muh. Arsan Akbar

# Project Overview
With the advancement of information technology, many library book management applications now provide various book collections in digital format that can be accessed online. This accessibility drives the need for smarter book search features, one of which is through the implementation of recommendation systems. A recommendation system functions to help users find books that match their interests and preferences by providing suggestions based on specific inputs or criteria they determine. As explained by Murti et al. (2019), a recommendation system is a technique that aims to suggest the most relevant choice items for users.

For example, in the E-Library application at the Banyuwangi State Polytechnic Library, a search system has been built to facilitate users in finding desired books. However, in practice, it was found that search accuracy is sometimes less than optimal. For instance, when users type a keyword in the form of a complete book title, the system is not always able to display appropriate results, even displaying an "Empty Data" message, even though the book is actually available in the database. This problem demonstrates the importance of using more effective search methods or algorithms to improve the performance of recommendation systems (Sadesty Rahmadhani et al. 2024).

Related to this problem, this project develops a book recommendation system based on Content-Based Filtering and Collaborative Filtering using the Book-Crossing dataset. This project aims to optimize the book search and discovery process by providing more relevant recommendations based on book content and previous user interaction patterns. With this approach, it is expected that users can receive book suggestions that match their preferences, even without entering exact keywords. In addition, this project also examines the application of machine learning and deep learning-based models to improve the accuracy and quality of the generated recommendations.

Referensi: 

[Murti, H., Lestariningsih, E., & ., S. (2019). PERANCANGAN SISTEM REKOMENDASI BUKU PADA KATALOG PERPUSTAKAAN MENGGUNAKAN PENDEKATAN CONTENT-BASED FILTERING DAN ALGORITMA FP-GROWTH. SINTAK, 3. Retrieved from https://www.unisbank.ac.id/ojs/index.php/sintak/article/view/7643](https://www.unisbank.ac.id/ojs/index.php/sintak/article/view/7643)

[Sadesty Rahmadhani, Lutfi Hakim, & Galih Hendra Wibowo. (2024). Sistem Rekomendasi Penelusuran Buku Berbasis Content-Based Filtering dengan Pembobotan TF-RF. Jurnal Informatika Polinema, 10(4), 491–500. https://doi.org/10.33795/jip.v10i4.5565](https://jurnal.polinema.ac.id/index.php/jip/article/view/5565)

---

# Business Understanding
## Problem Statements
- Based on user data, how to build a personalized book recommendation system using Content-Based Filtering techniques?
- By utilizing available rating data, how can the system recommend other books that users might like and have not read before?

## Goals
- Generate personalized book recommendations according to user preferences using Content-Based Filtering techniques.
- Generate book recommendations that match user interests and have not been read before using Collaborative Filtering techniques.

## Solution Approach
### Content-Based Filtering
This approach recommends books based on content similarity between books already known to be liked by users and other books in the collection. The algorithm used is Word2Vec to build vector representations of book features. The model is then evaluated with a manual precision calculation approach to measure the relevance of generated recommendations.
### Collaborative Filtering 
This approach recommends books by utilizing interaction patterns between users. The system will identify other users with similar behavior patterns and recommend books liked by those users. The technique used is matrix factorization, specifically the Singular Value Decomposition (SVD) method, to predict ratings or interest in books. To optimize the model, hyperparameter tuning is performed using the grid search method to obtain the best parameter combination. Model evaluation is performed using the Root Mean Squared Error (RMSE) metric to measure rating prediction accuracy.

---

# Data Understanding
The dataset used in this project is the Book-Crossing Dataset. This dataset contains information about users, books, and ratings given by users to books. This dataset can be downloaded via the following link: 
[Book-Crossing Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset?select=Ratings.csv) or https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset?select=Ratings.csv

## Data Quantity and Condition
- The user dataset contains 278,858 user data with 3 columns. The Age column has many missing values (approximately 40% of data is unavailable).
- The book dataset contains 271,360 book data with 8 columns. There are some missing values in the Book-Author, Publisher, and Image-URL-L columns.
- The ratings dataset contains 1,149,780 book rating data with 3 columns and no missing values were found in this table.

This dataset consists of three main tables: Users, Books, and Ratings, which are interrelated through User-ID and ISBN.

## Dataset Variables

| Dataset  | Variable              | Description |
|:---------|:----------------------|:----------|
| Users    | User-ID                | Unique ID for each user |
|          | Location               | User location|
|          | Age                    | User age |
| Books    | ISBN                   | Book ISBN number |
|          | Book-Title             | Book title. |
|          | Book-Author            | Book author name|
|          | Year-Of-Publication    | Book publication year. |
|          | Publisher              | Book publisher name. |
|          | Image-URL-S            | Small-sized cover image URL. |
|          | Image-URL-M            | Medium-sized cover image URL. |
|          | Image-URL-L            | Large-sized cover image URL. |
| Ratings  | User-ID                | ID of user providing rating. |
|          | ISBN                   | ISBN of rated book. |
|          | Book-Rating            | Rating given (0 for implicit, 1-10 for explicit rating). |

# Exploratory Data Analysis (EDA) - Univariate Exploratory Data Analysis

## Book Dataset
- The ISBN variable is a unique code used to identify each book individually. In this dataset, all 271,360 rows have unique ISBN values with no missing values.
- The Book Title variable is the book title. Of the 271,360 data points, there are 242,135 unique titles, indicating some books have the same title (possibly different editions or republications). The most frequently appearing titles include Selected Poems, Little Women, and Wuthering Heights.
- The Book Author variable displays the author name for each book. There are 102,022 unique authors and only 2 missing values. The most frequently appearing authors in this dataset are Agatha Christie, William Shakespeare, and Stephen King, each with hundreds of titles.
- The Year of publication variable is the book publication year which was originally object type and has been converted to numeric. The range is very wide (0 to 2050), indicating outliers or data errors. The most common years are between 1998 and 2002.
- The Publisher variable shows the book publisher. There are 16,807 unique publishers with only 2 missing data points. Some of the most frequently appearing publishers are Harlequin, Silhouette, and Pocket.
- The Image URL variable is the cover image URL in three different sizes. This column has no missing values, except Image-URL-L which has 3 missing values. This data is supplementary and not very necessary for this recommendation system.

## User Dataset
- The User-ID variable has 278,858 unique values in the User-ID column, indicating that each user in the dataset is unique. No missing values were found in this column.
- The Location variable has 57,339 unique locations in the Location column, showing the geographical diversity of users. The most frequently appearing location is London, England, United Kingdom appearing 2,506 times, followed by Toronto, Ontario, Canada and Sydney, New South Wales, Australia. No missing data was found in this column.
- The Age variable shows that of the total data, only 168,096 (about 60%) have age values. The average user age is about 35 years with a standard deviation of 14.43 years. There are extreme values such as minimum age of 0 years and maximum of 244 years which are likely input errors or outliers.

## Rating Dataset
- The User-ID variable has 105,283 unique users in this dataset, and no missing values were found in the User-ID column.
- The ISBN variable shows 340,556 unique ISBNs are recorded. The most frequently appearing ISBN is 0971880107 appearing 2,502 times, followed by 0316666343 appearing 1,295 times. There are no missing values in this column.
- The Book Rating variable has 1,149,780 entries with an average value of 2.87 and standard deviation of 3.85. Rating values range from 0 to 10. As many as 716,109 entries (about 62%) have a rating of 0, which usually indicates no rating was given. Rating 10 appears 78,610 times, showing some users gave the maximum score. The other distribution shows an increase in data as ratings increase, especially from values 5 to 8.

## Variables Requiring Fixes

| Dataset          | Variable               | Problem Detected                                | Preprocessing Action                                 |
|------------------|------------------------|--------------------------------------------------|--------------------------------------------------------|
| Book             | Book-Title            | Duplicate book titles exist                         | Remove duplicate book titles                        |
| Book             | Book-Author            | 2 missing values exist                         | Remove rows with empty values                        |
| Book             | Publisher              | 2 missing values exist                         | Remove rows with empty values                        |
| Book             | Year-Of-Publication    | Invalid values: 0 and > 2025                  | Replace with median value |
| Book             |  Image-URL | Variable not needed                  | Remove variable (drop) |
| User         | Age                    | Outlier values: 0 and > 100                       | Replace with median value |

--------------------------------------------------------------------------------

# Data Preparation

At this stage, a series of data cleaning and preparation processes are performed before entering the modeling stage. Data preparation is important to ensure that the data used is clean, consistent, relevant, and ready to support model performance. The following are the data preparation steps taken:

## Handling Missing Value

### Book-Author and Publisher

Data rows that have missing values in the `Book-Author` or `Publisher` columns are removed because both attributes contain important information used in content-based recommendation systems. The loss of this information can cause a decrease in recommendation accuracy.

```python
book = book[book['Book-Author'].notnull()]
book = book[book['Publisher'].notnull()]
```

### Year-Of-Publication

Invalid `Year-Of-Publication` values (valued 0 or more than 2025) are replaced with `NaN`, then filled using the median of valid publication years. This is done to maintain data consistency and avoid bias in the publication year feature that can affect recommendation quality.

```python
book.loc[(book['Year-Of-Publication'] == 0) | (book['Year-Of-Publication'] > 2025), 'Year-Of-Publication'] = np.nan
median_year = book['Year-Of-Publication'].median()
book['Year-Of-Publication'].fillna(median_year, inplace=True)
```

### Age

User data with `Age` below 5 years or above 100 years is considered unrealistic and can be outliers that damage analysis. Therefore, unreasonable values are replaced with `NaN`, then filled with the median age so that the user age distribution remains representative.

```python
user.loc[(user['Age'] < 5) | (user['Age'] > 100), 'Age'] = np.nan
median_age = user['Age'].median()
user['Age'].fillna(median_age, inplace=True)
```

## Handling Duplicates

### Book-Title

Data duplication based on `Book-Title` can cause bias in the model training process, such as giving more weight to certain books. Therefore, duplicate data is removed so that the recommendation system is not biased.

```python
book = book.drop_duplicates(subset=['Book-Title']).reset_index(drop=True)
```

## Feature Reduction

### Removing Image Columns

The `Image-URL-S`, `Image-URL-M`, and `Image-URL-L` columns are not used in text and rating-based modeling processes. Removing irrelevant features helps reduce noise and speed up the model training process.

```python
book.drop(['Image-URL-S', 'Image-URL-M', 'Image-URL-L'], axis=1, inplace=True)
```

# Content-Based Filtering Preparation

## Text Data Feature Extraction

For the content-based filtering approach, a text representation of books is needed. Therefore, the `Book-Title`, `Book-Author`, and `Publisher` columns are combined into one `text_data` column, which is later processed further using text representation learning techniques.

```python
text_data = book['Book-Title'] + ' ' + book['Book-Author'] + ' ' + book['Publisher']
```

## Tokenization and Word2Vec Training

The Word2Vec model is trained using tokenized `text_data`. Word2Vec helps represent text in numerical vector form that captures semantic relationships between words, allowing the recommendation model to better understand content context.

```python
sentences = [text.split() for text in text_data]
model = Word2Vec(sentences, vector_size=100, window=5, min_count=1)
```

# Collaborative Filtering Preparation

## Filter Rating Data

Rating data filtering is performed to ensure that only data that appears frequently enough is used in training. Users or books with few ratings provide less useful information for building a reliable collaborative filtering model.

```python
filtered_ratings = rating[
    (rating['User-ID'].isin(rating['User-ID'].value_counts()[rating['User-ID'].value_counts() > 20].index)) &
    (rating['ISBN'].isin(rating['ISBN'].value_counts()[rating['ISBN'].value_counts() > 20].index))
]
```

## Encode Label

Rating data needs to be converted into a standard format that can be processed by the recommendation library (in this case Surprise). Label encoding ensures that `User-ID` and `ISBN` are in numeric format, and the rating scale is properly defined (0-10).

```python
reader = Reader(rating_scale=(0, 10))
data = Dataset.load_from_df(filtered_ratings[['User-ID', 'ISBN', 'Book-Rating']], reader)
```

## Split Data

Data is split into training set (`trainset`) and test set (`testset`) to enable objective model validation. By separating data, we can evaluate model performance on previously unseen data.

```python
trainset, testset = train_test_split(data, test_size=0.2, random_state=42)
```


## Results After Data Preparation

| Dataset        | Data Count | Features                                           | Notes                                      |
|----------------|-------------|-------------------------------------------------|-------------------------------------------------|
| Books          | 242,132     | ISBN, Book-Title, Book-Author, Year-Of-Publication, Publisher | No missing values in all columns |
| Users          | 278,858     | User-ID, Location, Age                         | No missing values after median filling in Age column |

---

# Modeling and Results

At this stage, the recommendation system is developed using two different approaches: **Content-Based Filtering** and **Collaborative Filtering**. Each approach generates a Top-N book recommendation list for users, based on the working mechanism and model parameters that have been determined.

---

## 1. Content-Based Filtering with Word2Vec and Cosine Similarity

In the **Content-Based Filtering** approach, the recommendation system is built based on content similarity between books. Book representation is formed using **Word2Vec** techniques, and similarity between books is calculated using **cosine similarity**.

### Model Working Method

- Combine text from `Book-Title`, `Book-Author`, and `Publisher` columns into one combined text column.
- Tokenize text into word lists.
- Train the **Word2Vec** model to generate vector representation for each word with main parameters:
  - `vector_size=100`: Embedding vector dimension.
  - `window=5`: Context window size.
  - `min_count=1`: Words with minimum frequency of 1 are included.
- For each book, generate vector representation by averaging its word vectors.
- Measure similarity between books using **cosine similarity**.
- Compile **Top-5** book recommendations with highest cosine similarity scores against input books.

### Recommendation Output (Example)

| ISBN        | Title                    | Author            | Publisher                | Similarity Score |
|-------------|---------------------------|-------------------|---------------------------|------------------|
| 0393045218  | The Mummies of Urumchi     | E. J. W. Barber   | W. W. Norton & Company    | 1.00000          |
| 3791535714  | Die Schildbürger           | Erich Kästner     | Dressler Verlag           | 0.99991          |
| 033037401X  | Midwinter of the Spirit    | Phil Rickman      | Pan Publishing            | 0.99990          |
| 0965813509  | The Throne of Bones        | Brian McNaughton  | Terminal Fright           | 0.99990          |
| 0316734500  | The Bookseller of Kabul    | Asne Seierstad    | Little, Brown             | 0.99990          |

### Advantages
- Can provide recommendations based only on book content, without relying on other user data.
- Suitable for addressing the **cold-start** problem for new books.

### Disadvantages
- Can only recommend books similar to already known books.
- Recommendation quality depends on completeness and accuracy of book metadata.

---

## 2. Collaborative Filtering with SVD (Singular Value Decomposition)

In the **Collaborative Filtering** approach, the recommendation system is developed using **Matrix Factorization** techniques with the **Singular Value Decomposition (SVD)** algorithm. This model utilizes interaction patterns (ratings) between users and items.

### Model Working Method

- Use the **Surprise** library to process `User-ID`, `ISBN`, and `Book-Rating` datasets.
- Split dataset into **trainset** and **testset** with an 80:20 ratio.
- Perform **Grid Search** to find the best parameter combination with evaluation using **Root Mean Square Error (RMSE)**:
  - `n_factors`: Number of latent factors (experiment 50 and 100).
  - `lr_all`: General learning rate (experiment 0.005 and 0.01).
  - `reg_all`: General regularization (experiment 0.02 and 0.1).
- Train the best **SVD** model based on Grid Search results on training data.
- Use trained model to predict ratings for books not yet read by users.
- Compile **Top-5** book recommendations with highest predicted ratings.

### Recommendation Output (Example)

#### Sample User ID: 11676

| ISBN         | Book Title                                                   | Book Author         | Year of Publication | Publisher                 | Predicted Rating |
|--------------|---------------------------------------------------------------|---------------------|---------------------|----------------------------|------------------|
| 193156146X   | The Time Traveler's Wife                                       | Audrey Niffenegger  | 2003.0              | MacAdam/Cage Publishing    | 10.00            |
| 0553582143   | Body of Lies                                                   | Iris Johansen       | 2003.0              | Bantam                    | 10.00            |
| 0345413881   | Dr. Death (Alex Delaware Novels (Paperback))                   | Jonathan Kellerman  | 2001.0              | Ballantine Books           | 10.00            |
| 0380720132   | The Mystery of the Cupboard (Indian in the Cupboard Adventures) | Lynne Reid Banks    | 1999.0              | HarperTrophy               | 9.99             |
| 2253044903   | Le Parfum : Histoire d'un meurtrier                             | Patrick Süskind     | 1988.0              | LGF                        | 9.95             |

### Advantages
- Can discover hidden relationships between books from user rating patterns.
- Provides more diverse recommendations compared to content-based filtering.

### Disadvantages
- Requires sufficient user interaction data for optimal performance.
- Less effective in **cold-start** cases for new users or new books.

---

# Evaluation

At this evaluation stage, metrics appropriate to each recommendation model approach are used.

## 1. Content-Based Filtering Evaluation (Word2Vec)

In the Content-Based Filtering model using Word2Vec, evaluation is performed using the **Precision** method manually. **Precision** is used to measure the relevance level of recommendation results based on manual assessment.

### Metric Used: Precision (Manual Evaluation)

**Precision Formula:**

![Formula Precision](https://github.com/minggo-commits/book-recommendation/blob/main/Img/Formula%20Precision.PNG?raw=true)


Notes:
- **Relevant items**: Recommended items deemed appropriate based on content similarity (title, author, or publisher).
- **N**: Total number of recommendations evaluated.

### Evaluation Method
1. Take the top 5 recommendation results from the Content-Based Filtering model.
2. Manually assess the appropriateness of each recommendation with the initial input.
3. Calculate the ratio of the number of relevant recommendations to total recommendations.

### Evaluation Results

Example evaluation for input book **"The Mummies of Urumchi"**:

| Recommended Book Title                        | Relevant? |
|:---------------------------------------------------------|:--------:|
| The Mummies of Urumchi                                   | ✔️       |
| Die SchildbÃ?Â¼rger.                                 | ✔️       |
| Midwinter of the Spirit                | ✔️       |
| The throne of bones                                  | ✔️       |
| The Bookseller of Kabul                                  | ✔️       |

- **Number of relevant recommendations**: 5 out of 5
- **Precision**: **100%**

## 2. Collaborative Filtering Evaluation (SVD)

In the Collaborative Filtering model using the SVD (Singular Value Decomposition) algorithm, the **Root Mean Squared Error (RMSE)** evaluation metric is used.

### Metric Used: RMSE

**RMSE Formula:**

![Formula RMSE](https://github.com/minggo-commits/book-recommendation/blob/main/Img/Formula%20RMSE.PNG?raw=true)

Notes:

![RMSE Formula Description](https://github.com/minggo-commits/book-recommendation/blob/main/Img/Ket%20Formula%20RMSE.PNG?raw=true)

RMSE measures how far model predictions are from actual values; the smaller the RMSE, the better the model performance.

### Evaluation Results

- **Best RMSE** from Grid Search results: **3.486**.
- Best parameters obtained:
  - **n_factors**: 100
  - **lr_all**: 0.005
  - **reg_all**: 0.1
 

## Conclusion

- **Content-Based Filtering** produces very high Precision (100%), showing high accuracy in recommending similar books.
- **Collaborative Filtering (SVD)** provides fairly small RMSE, showing model accuracy in predicting user preferences based on rating patterns.

Both approaches have their respective advantages:
- Content-Based Filtering is more suitable for finding similar items from content.
- Collaborative Filtering is more effective for personalization based on user behavior.
