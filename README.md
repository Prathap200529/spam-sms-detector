# spam-sms-detector
spam sms detector using google Collab

# Spam SMS Detection

A brief description of what this project does and who it's for
this project is about spam sms detection model that is built using google collab.

this detector uses pre collected data to intepret and verify the sms.
## Deployment

To deploy this project run

```bash
  npm run deploy
```

from google.colab import drive
import pandas as pd
import csv

drive.mount('/content/drive')
file_url = '/content/drive/My Drive/spam sms detector/spam.csv'
data = pd.read_csv(file_url,usecols=[0, 1],encoding='latin1')
data.head()

import string
import nltk
nltk.download('stopwords')
nltk.download('punkt')

stopwords = nltk.corpus.stopwords.words('english')
punctuation = string.punctuation

print(stopwords[:5])
print(punctuation)

!pip install nltk
import nltk
import string

nltk.download('punkt')
nltk.download('stopwords')
nltk.download('punkt_tab') # Download the missing 'punkt_tab' data

stopwords = nltk.corpus.stopwords.words('english')
punctuation = string.punctuation

def pre_process(v2):
    remove_punct = "".join([word.lower() for word in v2 if word not in punctuation])
    tokenize = nltk.tokenize.word_tokenize(remove_punct)
    remove_stopwords = [word for word in tokenize if word not in stopwords]
    return remove_stopwords

data['processed'] = data['v2'].apply(lambda x: pre_process(x))

print(data['processed'].head())

def categorize_words():
    spam_words = []
    ham_words = []
    #handling messages associated with spam
    for v2 in data['processed'][data['v1'] == 'spam']:
        for word in v2:
            spam_words.append(word)
    #handling messages associated with ham
    for v2 in data['processed'][data['v1'] == 'ham']:
        for word in v2:
            ham_words.append(word)
    return spam_words, ham_words

spam_words, ham_words = categorize_words()

print(spam_words[:5])
print(ham_words[:5])

def predict(v2):
    spam_counter = 0
    ham_counter = 0
    #count the occurances of each word in the sms-v2 string
    for word in v2:
        spam_counter += spam_words.count(word)
        ham_counter += ham_words.count(word)
    print('***RESULTS***')
    #if the message is ham
    if ham_counter > spam_counter:
        accuracy = round((ham_counter / (ham_counter + spam_counter) * 100))
        print('messege is not spam, with {}% certainty'.format(accuracy))
    #if the message could be equally spam and ham
    elif ham_counter == spam_counter:
        print('message could be spam')
    #if the message is spam
    else:
        accuracy = round((spam_counter / (ham_counter + spam_counter)* 100))
        print('message is spam, with {}% certainty'.format(accuracy))

user_input = input("Please type a spam or ham message to check if function predicts accurately\n")

processed_input = pre_process(user_input)

predict(processed_input)
## Features

- simple and easy to use and understand since it has no complicated code.
- it as a data set of 5000 sms so it would compare and give accurate results.
- in the file url enter the url where the file is saved in your drive in my case i have created a folder specially for this project so i have named that path


## FAQ

#### Question 1) pre process not defined error

Answer 1)try to run entire code using ctrl+F9

