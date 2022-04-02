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

## Most frequently occured four-word sequences
```python
text6 = []
for i in range(len(text4)-3):
    four = text4[i] + ' ' + text4[i+1]+' '+text4[i+2] + ' '+text4[i+3]
    text6.append(four)

# ruling out sequences that contain a period
text6_noperiod = []
for i in text6:
    if re.findall('[.]', i) == []:
        text6_noperiod.append(i)

```
![image](https://user-images.githubusercontent.com/43469728/161374103-803e7fd0-0e7b-47e6-ad60-0255260e5f93.png)

- Make a list of four word sequences (that do not contain a period)
 
        
```python        
# most common four word sequence (not including a period)
four_seq_noperiod = Counter(text6_noperiod)
top5_four_seq_noperiod = four_seq_noperiod.most_common(5)
top5_four_seq_noperiod


# histogram
plt.figure(figsize = (30,10))
sns.set(font_scale = 2.5)
h = sns.barplot(x = 'words', y = 'count', data = seq4_freq_df.iloc[0:30])
h.set_xticklabels(h.get_xticklabels(),rotation = 90)
h.set_title('Top 30 Frequently occured four word sequences')
```
![image](https://user-images.githubusercontent.com/43469728/161373752-e964f22f-4ea8-4403-80a5-1a02d139d643.png)


