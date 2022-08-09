punctuation_chars = ["'", '"', ",", ".", "!", ":", ";", '#', '@']
def strip_punctuation(st):
    for i in punctuation_chars:
            st=st.replace(i,"")
    return st

positive_words = []
with open("positive_words.txt") as pos_f:
    for lin in pos_f:
        if lin[0] != ';' and lin[0] != '\n':
            positive_words.append(lin.strip())

def get_pos(st):
    str_free=strip_punctuation(st)
    str_lower=str_free.lower()
    list_str= str_lower.split()
    cpt=0
    for word in list_str :
            if word in positive_words:
                cpt += 1
    return cpt


negative_words = []
with open("negative_words.txt") as pos_f:
    for lin in pos_f:
        if lin[0] != ';' and lin[0] != '\n':
            negative_words.append(lin.strip())

def get_neg(st):
    str_free=strip_punctuation(st)
    str_lower=str_free.lower()
    list_str= str_lower.split()
    cpt=0
    for word in list_str :
            if word in negative_words:
                cpt += 1
    return cpt

twitter_data=open('project_twitter_data.csv','r')
data_lines=twitter_data.readlines()

results=open('resulting_data.csv','w')
results.write('Number of Retweets, Number of Replies, Positive Score, Negative Score, Net Score')
results.write('\n')

for line in data_lines[1:]:
    values=line.strip().split(',')#to a have a list of values (3 cases)
    row='{},{},{},{},{}'.format(values[1],values[2],get_pos(values[0]),get_neg(values[0]),get_pos(values[0])+get_neg(values[0])*-1)
    results.write(row)
    results.write('\n')
    
twitter_data.close()
results.close()
