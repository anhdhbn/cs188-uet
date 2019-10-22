# Câu 1

## Chung ta quan tâm đến 3 khía cạnh

- khoảng cách đến ghost gần nhất
- Khoảng cách đến thức ăn gần nhất
- số lượng thức ăn, giúp nó ăn hết thức ăn.

## 1.1

``` python
if len(currentGameState.getCapsules()) != 0:
  closestCapsule = min([manhattanDistance(capsule, newPos) for capsule in currentGameState.getCapsules()])
  if closestCapsule == 0:
    capsuleScore = 1000
  else:
    capsuleScore += 100/closestCapsule
```

- Đoạn này kiểm tra xem có thức ăn đặc biệt không
  - Nếu có thì nếu ăn thức ăn đặc biệt thì điểm tăng 1000
  - Ngược lại tăng 100/(khoảng cách đến thức ăn đặc biệt) giúp cho pacman tiến đến để ăn thức ăn đặc biệt.

## 1.2

``` python
foodList = newFood.asList()
if len(foodList) != 0:
  closestfood = min([manhattanDistance(newPos, food) for food in foodList])
else:
  closestfood = 0
```

- tính khoảng cách manhattan từ vị trí hiện tại đến tất cả các food và lấy giá trị nhỏ nhất

## 1.3

``` python
if(ghost.scaredTimer != 0):
  if closestghost <= 1:
    ghostDistance += 500
  else:
    ghostDistance = 500/closestghost
    score = (-1.4 * closestfood) + ghostDistance - (2*len(foodList)) + capsuleScore
else:
  if closestghost <= 1:
    ghostDistance -= 1000
  else:
    ghostDistance = - 15/closestghost
    score = (-1.4 * closestfood) + ghostDistance - (100*len(foodList)) + capsuleScore
```

- Nếu đang trong thời gian con ghost sợ
  - thì càng gần con ghost điểm càng cao, mục đích là để ăn được con ghost
- Nếu ___không___ trong thời gian con ghost sợ
  - thì càng gần con ghost điểm càng thấp, mục đích để không bị chết

## Câu 2

``` python
def minimaxSearch(state, agentIndex, depth):
  # Neu la min layer va la ghost cuoi dung
  if agentIndex == state.getNumAgents():
    # neu la max depth
    if depth == self.depth:
      return self.evaluationFunction(state)
    else:
      # tao max layer voi depth = depth + 1
      return minimaxSearch(state, 0, depth + 1)
  else:
    moves = state.getLegalActions(agentIndex)
    if len(moves) == 0:
      return self.evaluationFunction(state)
    next = [minimaxSearch(state=state.generateSuccessor(agentIndex, move), agentIndex=(agentIndex +1), depth=depth) for move in moves]
    if(agentIndex == 0):
      return max(next)
    else:
      return min(next)
```

- ```agentIndex == state.getNumAgents()```: nếu đây là layer min và là ghost cuối cùng
  - ```if depth == self.depth:```: nếu là max depth thì return hàm đánh giá
  - Ngược lại đề quy với ```agentIndex``` là 0 (là pacman) và độ sâu tăng thêm 1
- ```if(agentIndex == 0):```
  - Nếu là pacman thì return max
  - Người lại thì return min

## Cau 3

``` python
def nodeMax(state, agentIndex, depth, a, b):
  if depth > self.depth:
    return self.evaluationFunction(state)
  temp_a = -9999999
  for action in state.getLegalActions(agentIndex):
    currentNode = nodeMin(state.generateSuccessor(agentIndex, action), agentIndex + 1, depth, a, b)
    temp_a = max(temp_a, currentNode)
    # tren node Max la node Min
    # Node min se update gia tri b
    # Neu a < b thi return luon a cat nhanh con lai
    if b is not None and temp_a > b:
      return temp_a
    # nguoc lai, cap nhat gia tri max cua a
    a = max(a, temp_a)
  if temp_a != -9999999:
    return temp_a
  else:
    return self.evaluationFunction(state)
```

- ```if depth > self.depth:``` kiểm tra độ sâu, nếu đạt độ sâu giới hạn thì return hàm đánh giá
- ```if b is not None and temp_a > b:``` Nếu a < b thì return luôn, cắt nhánh luôn
- ```if temp_a != -9999999:``` nếu temp_a không đổi thì return hàm đánh giá

## Câu 4

``` python
def expectimaxSearch(state, agentIndex, depth):
  # Neu la min layer va la ghost cuoi dung
  if agentIndex == state.getNumAgents():
    # neu la max depth
    if depth == self.depth:
      return self.evaluationFunction(state)
    # tao max layer voi depth = depth + 1
    else:
      return expectimaxSearch(state, 0, depth + 1)
  else:
    moves = state.getLegalActions(agentIndex)
    if len(moves) == 0:
      return self.evaluationFunction(state)
    next = [expectimaxSearch(state.generateSuccessor(agentIndex, m), agentIndex + 1, depth) for m in moves]

    if agentIndex == 0:
      return max(next)
    else:
      # khong dung min nua ma dung avg
      return sum(next) / len(next) 

```

> Câu này tương tự câu 2 phần minimax, chỉ khác ở chỗ đối với con ghost chúng ta ko ```return min(next)``` nữa mà ```return sum(next) / len(next)``` tức là tính trung bình.

## Cau 5

> Ở câu 5 này em làm tương tự câu 1, em chỉ chỉnh tham số để đạt được điểm tối đa của autograder thôi ạ.