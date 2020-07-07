# :scroll: Korean community comment Dataset  
DC인사이드의 15000여개의 댓글데이터셋  
15,000 comment data parsed in the Korean community  
Project Date :calendar: `2020-06-20`  
  
![label_g](https://user-images.githubusercontent.com/55491354/86750801-2d314180-c079-11ea-9c73-2d3b6876393b.png)   
###### 전체 데이터중 18%정도의 악성댓글을 가지고 있는 데이터셋 입니다(2-way 기준)
----
# 1.Dataset_Class
Class | Description
---- | ---- 
Text | 원문 텍스트입니다
Malignant index | 악성지수 입니다 0~2의 값으로 부여되어 있습니다

# 1-1.Malignant index
Malignant index | Description
---- | ---- 
0 | 게시되어도 전혀 문제가 없는 댓글 입니다.
1 | 비속어를 사용하지 않았지만 악성댓글이라 판단하기에 부족함이 없는 댓글 입니다.
2 | 비속어를 사용하고 명백하게 악성댓글이라 판단이 가능한 댓글입니다

3-way Classification로 작성되어 있지만
학습결과 Binary Classification 형태로 데이터셋을 사용하면 정확도가 잘 나오기 때문에
아래에서 설명하는 함수로 데이터를 재가공하여 사용하는것을 추천드립니다

# 1-2.Rework malicious index(3-way -> 2-way)
malicious index를 Binary Classification 형태로 변경하는 기준은 두가지입니다.

## Low level (malicious index Value Change 1 -> 0, 2 -> 1)
```
def Row_rework_label(data): #Binary Classification (Low level)  
count = 0
    for i in data:
        if(i==2):
            data[count] = 1
        elif(i==1):
            data[count] = 0
        count = count+1
    return data
````
malicious index가 1인 경우엔 0으로 수정하는, 낮은 엄격도를 가지는 수정방법입니다

## High level (malicious index Value Change 1 -> 0, 2 -> 1)
```
def High_rework_label(data): #Binary Classification 통합 (high level) 
count = 0
    for i in data:
        if(i==2):
            data[count] = 1
        count = count+1
    return data
````
malicious index가 1인 경우엔 0으로 수정하는, 높은 엄격도를 가지는 수정방법입니다  
## Dataset Load and Rework malicious index
````
dataset_csv = pd.read_csv('DCcomment.csv', names=['Text', 'label'])
X, Y = dataset_csv['Text'].values, dataset_csv['label'].values
#Y = High_rework_label(Y)
#Y = Row_rework_label(Y)
````
