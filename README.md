# Using-lemmatization-and-fuzzy-wuzzy-on-the-top-of-it-

## Remove stop words and use lemmatization - for trimming the word.

```
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer
from nltk.stem import LancasterStemmer
from nltk.stem import WordNetLemmatizer
nltk.download('punkt')
nltk.download('wordnet')
nltk.download('stopwords')
stop_words = set(stopwords.words('english'))
lemmatizer = WordNetLemmatizer()
ps = PorterStemmer()
ls=LancasterStemmer()
import nltk
nltk.download('averaged_perceptron_tagger')

```


## Using lematization on a column for geeting the original word and then using fuzzy wuzzy for geeting the match according to that word:

```
def get_suggestion(value, value_list, n):
    list_1=[]
    list_a=[]
    list_b=[]
    if value not in value_list:
        skill_tokenize=word_tokenize(value)
        lower_letter=[w.lower() for w in skill_tokenize]
        filtered_skill = [w for w in lower_letter if not w.lower() in stop_words] # ['product','management']
        filtered_words = [word for word in filtered_skill if word not in stopwords.words('english')]
        lemmatized_output = ' '.join([lemmatizer.lemmatize(word,pos='v') for word in filtered_words])
        result = process.extract(
            lemmatized_output,value_list , scorer=fuzz.token_set_ratio)
        for i in result:
            i=i[0]
            list_a.append(i)
        result = process.extract(lemmatized_output, list_a, scorer=fuzz.partial_ratio)
        for i in result:
            i=i[0]
            list_b.append(i)
            
        result = process.extract(lemmatized_output, list_b, scorer=fuzz.token_sort_ratio)

        return [i[0] for i in result if i[1]>70]
    return [value]
```
