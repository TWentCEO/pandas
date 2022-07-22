# Pandas

```sh
$pip install pandas
```

---

<img src ="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FN4NAc%2FbtqRoP1ml8o%2Fx4DD7ITezrXVcJKgEYR5a1%2Fimg.png" width =500 />

## 1. DataFrame 생성

![DataFrame](https://pandas.pydata.org/docs/_images/01_table_dataframe.svg)

```python
df = pd.DataFrame(
    {
        "Name" : [
        "Braund, Mr. Owen Harris",
        "Allen, Mr. William Henry",
        "Bonnell, Miss. Elizabeth",
        ],
        "Age" : [22, 35, 58],
        "Sex" : ["male", "male", "female"],
        }
    )
df
```

```
Name  Age     Sex
0   Braund, Mr. Owen Harris   22    male
1  Allen, Mr. William Henry   35    male
2  Bonnell, Miss. Elizabeth   58  female
```

---

## 2.Series

![Serise](https://pandas.pydata.org/docs/_images/01_table_series.svg)

데이터 프레임의 각 열은 'Seires'라고 한다.

```python
df["Age"]

#0    22
#1    35
#2    58
#Name: Age, dtype: int64

```

```python
ages = pd.Series([22, 35, 58], name="Age")

ages

#0    22
#1    35
#2    58
#Name: Age, dtype: int64
```

---

## 3. Data 정보 확인

```python
df.describe() # 숫자형 데이터의 정보를 보여줌.

# Age
# count   3.000000
# mean   38.333333
# std    18.230012
# min    22.000000
# 25%    28.500000
# 50%    35.000000
# 75%    46.500000
# max    58.000000
```

```python
df.info() # 데이터 프레임의 정보를 보여줌 (결측치, 데이터 타입 등)


<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3 entries, 0 to 2
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype
---  ------  --------------  -----
 0   Name    3 non-null      object
 1   Age     3 non-null      int64
 2   Sex     3 non-null      object
dtypes: int64(1), object(2)
memory usage: 200.0+ bytes

```

```py
df.shape

(3,3)
```

## 4. CSV 파일 불러오기

---

```py
titanic = pd.read_csv("data/titanic.csv")
```

```py
titanic.head(8)

PassengerId  Survived  Pclass                                               Name     Sex  ...  Parch            Ticket     Fare Cabin  Embarked
0            1         0       3                            Braund, Mr. Owen Harris    male  ...      0         A/5 21171   7.2500   NaN         S
1            2         1       1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  ...      0          PC 17599  71.2833   C85         C
2            3         1       3                             Heikkinen, Miss. Laina  female  ...      0  STON/O2. 3101282   7.9250   NaN         S
3            4         1       1       Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  ...      0            113803  53.1000  C123         S
4            5         0       3                           Allen, Mr. William Henry    male  ...      0            373450   8.0500   NaN         S
5            6         0       3                                   Moran, Mr. James    male  ...      0            330877   8.4583   NaN         Q
6            7         0       1                            McCarthy, Mr. Timothy J    male  ...      0             17463  51.8625   E46         S
7            8         0       3                     Palsson, Master. Gosta Leonard    male  ...      1            349909  21.0750   NaN         S
```

```python
titanic.dtypes  # columns의 데이터 타입을 확인

# PassengerId      int64
# Survived         int64
# Pclass           int64
# Name            object
# Sex             object
# Age            float64
# SibSp            int64
# Parch            int64
# Ticket          object
# Fare           float64
# Cabin           object
# Embarked        object
# dtype: object
```

---

## 5. Excel 파일 불러오기

```py
titanic.to_excel("titanic.xlsx", sheet_name="passengers", index=False) # 엑셀 파일로 저장
```

```py
titanic = pd.read_excel("titanic.xlsx", sheet_name="passengers") # 엑셀 파일 불러오기
```

---

## 6. Data 추출

```py
# columns 추출
age_sex = titanic[["Age","Sex"]]

age_sex.head()

	Age	    Sex
0	22.0	male
1	38.0	female
2	26.0	female
3	35.0	female
4	35.0	male
```

```py
above_35 = titanic[titanic["Age"] > 35]

above_35.head()

PassengerId  Survived  Pclass                                               Name     Sex  ...  Parch    Ticket     Fare Cabin  Embarked
1             2         1       1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  ...      0  PC 17599  71.2833   C85         C
6             7         0       1                            McCarthy, Mr. Timothy J    male  ...      0     17463  51.8625   E46         S
11           12         1       1                           Bonnell, Miss. Elizabeth  female  ...      0    113783  26.5500  C103         S
13           14         0       3                        Andersson, Mr. Anders Johan    male  ...      5    347082  31.2750   NaN         S
15           16         1       2                   Hewlett, Mrs. (Mary D Kingcome)   female  ...      0    248706  16.0000   NaN         S

```

```py
titanic["Age"] > 35

0      False
1       True
2      False
3      False
4      False
       ...
886    False
887    False
888    False
889    False
890    False
Name: Age, Length: 891, dtype: bool
```

```py
# titanic["Age"]중 Nan값이 아닌 row만 추출
age_no_na = titanic[titanic["Age"].notna()]

age_no_na.head()
PassengerId  Survived  Pclass                                               Name     Sex  ...  Parch            Ticket     Fare Cabin  Embarked
0            1         0       3                            Braund, Mr. Owen Harris    male  ...      0         A/5 21171   7.2500   NaN         S
1            2         1       1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  ...      0          PC 17599  71.2833   C85         C
2            3         1       3                             Heikkinen, Miss. Laina  female  ...      0  STON/O2. 3101282   7.9250   NaN         S
3            4         1       1       Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  ...      0            113803  53.1000  C123         S
4            5         0       3                           Allen, Mr. William Henry    male  ...      0            373450   8.0500   NaN         S

```

### loc / iloc

데이터 프레임 행, 컬럼 추출

- loc > 조건
- iloc > 범위

```py
titanic.loc[ titanic["Embarked"] == 'C', :] # Emkared가 C인 모든 row를 출력

titanic.loc[(titanic["Survived"] == 1) & (titanic["Sex"] == 'male')] # 생존한 사람들 중 남자를 모두 출력
```

---

## 7. 데이터 정렬

```py
titanic.sort_values('Age') # 'Age'값들을 올림차순으로 정렬
```

---

## 8. 데이터 처리

```py
## drop 함수를 통해 지정한 columns을 삭제
titanic.drop(["SibSp", "Parch"], axis=1)  # 변수를 지정해 주어야 드랍된 값이 저장됨.
```

```py
# apply함수에 lambda 를 이용해서 라벨링
titanic["Survived"] = titanic["Survived"].apply(lambda x : 'saved' if x == 1 else 'lost')
# map함수를 사용하여 Survived 라벨링
titanic["Survived"] = titanic["Survived"].map({"saved" : 1, "lost" : 0})
```

```py
x = titanic.dropna()  # 단, nan가 많을 경우 데이터 손실로 인해 사용하지 않음
x = titanic.fillna(0) # na값에 0을 채워넣는다. # 단, 0을 넣을시 데이터의 분포가 이상해져 훈련 결과에 영향을 미칠 수 있음.

titanic = titanic.fillna(titanic["Age"].mean()) # na값을 평균으로 바꾸는 것이 이상적일 경우가 많다.

titanic["Embarked"] = titanic["Embarked"].fillna("S") # 문자열의 na일 경우 가장 많은 값으로 바꿔주는 것이 좋다.
```

```py
titanic.isna().sum() # 결측치 확인

```

---

## 9. Group by

```py
data = {
    'city' : ['Busan', 'Busan', 'Busan', 'Busan', 'Seoul', 'Seoul', 'Seoul'],
    'fruit' : ['Apple', 'Orange', 'Banana', 'Banana', 'Apple', 'Apple', 'Banana'],
    'price' : [100, 200, 250, 300, 150, 200, 400],
    'quantity' : [1, 2, 3, 4, 5, 6, 7]
}

df = pd.DataFrame(data)

df

        city	fruit	price	quantity
0	    Busan	Apple	100	        1
1	    Busan	Orange	200	        2
2	    Busan	Banana	250	        3
3	    Busan	Banana	300	        4
4	    Seoul	Apple	150	        5
5	    Seoul	Apple	200	        6
6	    Seoul	Banana	400	        7
```

```py
df.groupby('city').price.mean() #city 속성값들을 그룹화 한후 평균값


city
Busan    212.5
Seoul    250.0
Name: price, dtype: float64
```

```py
df.groupby(['city', 'fruit']).mean() # city를 우선적으로 그룹화 한뒤 fruit값을 그룹화 하여 각 과일의 다른 속성값에 대한 평균값을 구함

                price	quantity
city	fruit
Busan	Apple	100.0	1.0
        Banana	275.0	3.5
        Orange	200.0	2.0
Seoul	Apple	175.0	5.5
        Banana	400.0	7.0
```
