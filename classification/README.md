# CS188 - UET

Requirements <http://ai.berkeley.edu/classification.html>

## Question 1 (4 points): Perceptron

### Mô tả cách làm

Code hàm ```train``` trong **file** ```perceptron.py```
Dựa theo công thức trên trang chủ.

Thầy **Long** đã chữa câu này

**Hàm đã code**

``` python
def train( self, trainingData, trainingLabels, validationData, validationLabels ):
    # chi tiet trong code
    pass
```

### Kết quả

Accuracy: 74%
Passed: 4/4 Points

## Question 2 (1 point): Perceptron Analysis

### Mô tả cách làm

Code hàm ```findHighWeightFeatures``` trong **file** ```perceptron.py``` để lấy top 100 đặc trưng quan trọng nhất

Thầy **Long** đã chữa câu này

**Hàm đã code**

``` python
def findHighWeightFeatures(self, label):
    wy = self.weights[label]
    wy = sorted( [(v,k) for k,v in wy.items()], reverse=True )
    featuresWeights = [e[1] for e in wy[:100]]

    return featuresWeights
```

### Kết quả

Passed: 1/1 Points

## Question 3 (6 points): MIRA

## Mô tả cách làm

Code hàm ```TrainAndTune``` trong **file** ```mira.py```  
Khi cập nhật giá trị  trọng số trong trường hợp ytrue != ypred chúng ta cần cộng trừ ```f``` nhân với ```tau``` trong đó ```tau``` được tính với công thức như trên web, kèm theo một ngưỡng ```C``` để ```tau``` không quá lớn

Thầy **Long** đã chữa câu này

**Hàm đã code**

``` python
def trainAndTune(self, trainingData, trainingLabels, validationData, validationLabels, Cgrid):
        max_acc, max_C, max_weights = max([self.get_accuracy_c_weight( c, trainingData, trainingLabels, validationData, validationLabels ) for c in Cgrid])
        self.weights = max_weights

def get_accuracy_c_weight(self, c, trainingData, trainingLabels, validationData, validationLabels ):
    # Chi tiet trong code
    pass

def get_tau(self, c, wypred, wytrue, f):
    # Chi tiet trong code
    pass

def get_accuracy(self, weights, validationData, validationLabels):
    # Chi tiet trong code
    pass
```

### Kết quả

Accuracy: 82%
Passed: 6/6 Points

## Question 4 (6 points): Digit Feature Design

### Mô tả cách làm

Code hàm ```EnhancedFeatureExtractorDigit``` trong file ```dataClassifier.py```
Tính số lượng vùng trong lưới features, bằng cách gọi đệ quy 4 hướng pixArray()
Có thể có 1, 2, 3 vùng, đó là là 1 feature để tăng độ chính xác

**Hàm đã code**

``` python
"*** YOUR CODE HERE ***"
def enhancedFeatureExtractorDigit(datum):
    regions = 0
    marked = [[0 for j in range(DIGIT_DATUM_HEIGHT)] for i in range(DIGIT_DATUM_WIDTH)]
    for x in range(DIGIT_DATUM_WIDTH):
        for y in range(DIGIT_DATUM_HEIGHT):
            if(datum.getPixel(x,y) == 0 and marked[x][y] == 0):
                marked = pixArray(datum, x, y, marked)
                regions += 1
    features[1] = 0
    features[2] = 0
    features[3] = 0
    features[regions] = 1
    return features


def pixArray(datum, x, y, marked):
    # Chi tiet trong code
    pass
```

### Kết quả

Accuracy: 84%
Passed: 6/6 Points

## Question 5 (4 points): Behavioral Cloning

### Mô tả cách làm

Code hàm ```train``` trong file ```perceptron_pacman.py```  
Thực ra câu này tương tự cách làm câu 1

**Hàm đã code**

``` python
f = trainingData[i][0]
(score, label) = max([(f[label] * self.weights, label) for label in f.keys()])

if(label != trainingLabels[i]):
    self.weights += f[trainingLabels[i]]
    self.weights -= f[label]
```

### Kết quả

Accuracy: 72% (Passed, 4/4 Points)

## Question 6 (4 points): Pacman Feature Design

### Mô tả cách làm

Code **hàm** ```EnhancedPacmanFeatures``` trong file ```dataClassifier.py```  

- StopAgent: Pacman chỉ đứng im
- FoodAgent: Pacman chỉ hướng tới ăn đồ ăn, không quan tâm với môi trường xung quanh
- SuicideAgent: Pacman chỉ di chuyển hướng tới con ghost gần nhất bất kể nó sợ hay không sợ
- ContestAgent: Pacman từ p2 tránh ma, ăn thức ăn và ăn viên làm cho con ma sợ

Phần này thực chất là sáng tạo ra những feature cho pacman
Các feature đã tạo ra:

- nearest_ghost: khoảng cách gần nhất đến ghost
- nearest_capsule: khoảng cách gần nhất đến thức ăn to
- capsule_count: số lượng thức ăn to
- ("capsule", i): sắp xếp theo thứ tự sao cho càng gần thức ăn to điểm càng cao
- food: càng gần thức ăn điểm càng cao
- scare_times: thời gian sợ của con ma


**Hàm đã code**

``` python
def enhancedPacmanFeatures(state, action):
    # chi tiết trong code
    pass
```

## Kết quả

ContestAgent: 93%, Passed (2 Points)
SuicideAgent: 94%, Passed (2 Points)
Passed: 4/4 Points