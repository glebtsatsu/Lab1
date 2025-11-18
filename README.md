import urllib.request

url = 'http://dfedorov.spb.ru/python3/sport.txt'
response = urllib.request.urlopen(url)
data = response.read().decode('cp1251')

with open('sport.txt', 'w', encoding='utf-8') as f:
    f.write(data)

sports = []
objects = []

# уникальные виды
with open('sport.txt', 'r', encoding='utf-8') as f:
    for line in f:
        if line[-1] == '\n':
            line = line[:-1]
        l = line.split('\t')
        if len(l) > 1:
            if ',' in l[3]:
                count_sport = l[3].replace(' ', '').split(',')
                for i in range(len(count_sport)):
                    if count_sport[i].lower() not in sports:
                        sports.append(count_sport[i].lower())
            else:
                count_sport = l[3].replace(' ', '').lower()
                if count_sport not in sports:
                    sports.append(count_sport)

# колво obj
objects = [0] * len(sports)
with open('sport.txt', 'r', encoding='utf-8') as k:
    for line in k:
        if line[-1] == '\n':
            line = line[:-1]
        l = line.split('\t')
        if len(l) > 1:
            count_sport = l[3].replace(' ', '').split(',')
            for i in range(len(count_sport)):
                for j in range(len(sports)):  
                    if count_sport[i].lower() == sports[j] and count_sport[i] != '':
                        objects[j] += 1

# сравнение obj
max1 = max2 = max3 = 0
maxs1 = maxs2 = maxs3 = 0
for i in range(len(objects)):
    if objects[i] > max1:
        max3 = max2
        max2 = max1
        max1 = objects[i]
        maxs3 = maxs2
        maxs2 = maxs1
        maxs1 = i
    elif objects[i] > max2:
        max3 = max2
        max2 = objects[i]
        maxs3 = maxs2
        maxs2 = i
    elif objects[i] > max3:
        max3 = objects[i]
        maxs3 = i

print('First popular sport: ', (f"{sports[maxs1]},"), max1)
print('Second popular sport: ', (f"{sports[maxs2]},"), max2)
print('Third popular sport: ', (f"{sports[maxs3]},"), max3)
