import csv
import re
import string

from collections import defaultdict
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize


"""
Returns a list of data List[List[str]], where each entry represents a row in the csv file. 
The data is somewhat cleaned, i.e. removed numbers, trailing spaces, etc. but not yet tokenized
"""
def get_data(filename):
    data = []

    with open(filename, newline='') as csvfile:
        reader = csv.reader(csvfile, delimiter=',')      
        for row in reader:  # row is a list of text
            row = row[1:len(row)]  # remove the timestamp column
            #if (len(row) != 10) or (len(row) != 5):  # sanity check if number 1 to 10 are all present
             #   raise Exception('One of the rows is not valid' + str(len(row)))
            
            for i, col in enumerate(row):
                # remove the trailing numbers, different delimiters are used so account for those
                row[i] = re.split('[|.:)]', col, maxsplit=1)[-1]  # get the one without the number
                row[i] = row[i].strip()  # remove trailing spaces
            
            data.append(row)

    return data

"""
Clean the given data by removing non-essential stuff such as punctuation, common words, and standardizing them 
such as converting to lowercase. 
@data: List[List[str]] where each entry in the outer list is a response in csv, and each inner list is the response from 1 to 10
@return: None. Modifies in place the @data input to List[List[Tuple[str]]] where each string response is tokenized into essential keywords in list form
E.g. :
[
    [ # student 1
        ("course", "hard", "no", "time"),  # 1st response from student 1
        ("financial", "stress")  # 2.
        ...  # 3., 4, ... 10.
    ],
    [ # student 2
        ...
    ]
    ...
]
"""
def clean_data(data):
    # for cleaning purpose later
    stop_words = set(stopwords.words('english'))  
    for row in data:
        for i, response in enumerate(row):
            tokens = word_tokenize(response)
            # remove punctuation from each word
            table = str.maketrans('', '', string.punctuation)
            stripped = [w.translate(table) for w in tokens]
            tokens = [w.lower() for w in stripped]
            # # filter out stop words
            words = [w for w in tokens if not w in stop_words] 
            words = tuple(filter(None, words))  # remove empty strings
            row[i] = words

def determine_weights(data):
    shared_words = []
    weights = []

    for num,person in enumerate(data):
        weights.append([])
        for entry in person:
            for word in entry:
                for compare_num,compare_person in enumerate(data):
                    try:
                        weights[num][compare_num] = 0
                    except:
                        weights[num].append(0)
                    if compare_num != num:
                        for compare_entry in compare_person:
                            shared_response = False
                            if word in compare_entry:
                                shared_response = True
                                if word not in shared_words:
                                    shared_words.append(word)
                            if shared_response:
                                weights[num][compare_num] += 1
                    else:
                        pass
    return weights


def count_data(data):
    before = defaultdict(int)
    after = defaultdict(int)
    for row in data:
        after_networking, before_networking = row[:5], row[5:]
        for response in before_networking:
            before[response] += 1
        for response in after_networking:
            after[response] += 1
    
    print(before.values())
    print('Before networking:')
    for key, value in before.items():
        if value > 1:
            print('{}: {}'.format(key, value))
    print()
    print('After networking:')
    for key, value in after.items():
        if value > 1:
            print('{}: {}'.format(key, value))
                     
               
def main():
    data = get_data('data.csv')
    clean_data(data)
    try:
        count_data(data)
    except:
        pass
    weights = determine_weights(data)
    return weights
    """Final output
    Array = [Person1[0,Person2Val,Person3Val...],Person2[Person1Val,0,Person3Val...etc]]
    """

if __name__ == '__main__':
    main()
