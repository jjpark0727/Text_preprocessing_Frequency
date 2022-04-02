# Text_preprocessing_Frequency

I tried to preprocess and calculate the frequencies of the words/sequences of the words without using NLP packages (like nltk).


## Data load
```python
import csv
text = []
with open('data1.csv', newline='', encoding='utf-8') as f:
    reader = csv.reader(f)
    for row in reader:
        text.append(row)
```

![image](https://user-images.githubusercontent.com/43469728/161373972-e41c5612-9f82-4b10-b589-9839751390c6.png)

- Every line (row) was read from the file and values from every cell from the row was added as an element into the list

---



## Text preprocessing
```python
import re
# remove empty lines
text1 = []

for i in range(len(text)):
    for j in range(len(text[i])):
        if text[i][j] != '':
            text1.append(text[i][j])

# remove numbers and special characters, except for period
text2 = [re.sub('[^a-zA-Z.]', ' ', item).strip() for item in text1]

# add a space before every period
text3 = [re.sub('[.]', ' .', item).strip() for item in text2]

```
- Blank elements are removed (text1)
- Numbers, special characters except for a period(.) are removed (text2)
- Periods are considered as a word, so add a space before every period (text3)

---





## Word Frequency
```python
# split the element by space (more than one space)
text4 =[]
for item in text3:
    a = re.split("\s+", item)
    for i in a:
        text4.append(i)

# frequency
from collections import Counter
vocab = Counter(text4)
word_freq_df = pd.DataFrame(vocab.most_common())
word_freq_df.columns = ['words','count']


# histogram
sns.set(font_scale = 2.5)
plt.figure(figsize = (30,10))
h = sns.barplot(x = 'words', y = 'count', data = word_freq_df.iloc[0:30])
h.set_xticklabels(h.get_xticklabels(),rotation = 90)
h.set_title('Top 30 Frequently occured words')

```

![image](https://user-images.githubusercontent.com/43469728/161373891-be186ae6-4a50-4c32-99a0-14fbd2b53f89.png)

- Split the elements by (more than one) space (text4)
- Calculate the frequency of the words that occurred

![image](https://user-images.githubusercontent.com/43469728/161374200-3c7fad1d-f309-4ff6-acb6-ab135ade4d39.png)



---

## Most frequently occured two-word sequences
```python
text5 = []
for i in range(len(text4)-1):
    two = text4[i] + ' ' + text4[i+1]
    text5.append(two)

# ruling out sequences that contain a period
text5_noperiod = []
for i in text5:
    if re.findall('[.]', i) == []: # if the sequence does not contain a period
        text5_noperiod.append(i)
```
![image](https://user-images.githubusercontent.com/43469728/161374611-0aa1121b-d5cc-4d6f-8160-fe3d03d2236b.png)

- Make a list of two word sequences (that do not contain a period)
 
        
```python        
# most common two word sequence (not including a period)
two_seq_noperiod = Counter(text5_noperiod)
top10_seq_noperiod = two_seq_noperiod.most_common(10)
top10_seq_noperiod

seq_freq_df = pd.DataFrame(two_seq_noperiod.most_common())
seq_freq_df.columns = ['words','count']

#histogram
sns.set(font_scale = 2.5)
plt.figure(figsize = (30,10))
h = sns.barplot(x = 'words', y = 'count', data = seq_freq_df.iloc[0:30])
h.set_xticklabels(h.get_xticklabels(),rotation = 90)
h.set_title('Top 30 Frequently occured two word sequences')
```

![image](https://user-images.githubusercontent.com/43469728/161374646-ca247674-bfba-498d-b8be-afe4164450da.png)


### Reference
- https://wikidocs.net/21703
