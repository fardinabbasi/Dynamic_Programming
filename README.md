# Dynamic Programming
This repository contains a Python implementation for optimizing the selection of fruit boxes to maximize profit while adhering to constraints on weight and the number of boxes. This problem falls under the category of **multiple knapsack** problems.
## Problem Statement
You have 7 fruit boxes available in a warehouse that need to be sent to other cities. Due to posting constraints, you can send a maximum of 5 boxes with a total weight not exceeding 15 kg. Each box has a specific weight and price. The goal is to select the boxes to maximize the total profit.

| Box | Weight (Kg) | Price ($) |
| --- | --- | --- |
| 1 | 2 | 200 |
| 2 | 5 | 700 |
| 3 | 4 | 600 |
| 4 | 3 | 400 |
| 5 | 6 | 900 |
| 6 | 1 | 150 |
| 7 | 4.5 | 500 |

## Solution
The provided solution uses dynamic programming to solve this optimization problem. The approach involves creating a DP table to store the maximum profit for different combinations of boxes, weights, and quantities.

```python
class BoxOptimizer:
    def __init__(self, weights, prices, max_weight, max_boxes):
        self.weights = weights
        self.prices = prices
        self.max_weight = max_weight
        self.max_boxes = max_boxes
        self.n = len(weights)
        self.scale_factor = 10 ** max(len(str(weight).split('.')[-1]) if '.' in str(weight) else 0 for weight in weights)
        self.scaled_max_weight = int(max_weight * self.scale_factor)
        self.dp = [[[0 for _ in range(max_boxes + 1)] for _ in range(self.scaled_max_weight + 1)] for _ in range(self.n + 1)]

    def solve(self):
        for i in range(1, self.n + 1):
            for j in range(self.scaled_max_weight + 1):
                for k in range(1, self.max_boxes + 1):
                    w = int(self.weights[i - 1] * self.scale_factor)
                    if j >= w:
                        self.dp[i][j][k] = max(self.dp[i - 1][j][k], self.dp[i - 1][j - w][k - 1] + self.prices[i - 1])
                    else:
                        self.dp[i][j][k] = self.dp[i - 1][j][k]

        max_profit = self.dp[self.n][self.scaled_max_weight][self.max_boxes]

        result_weight = self.scaled_max_weight
        result_boxes = self.max_boxes
        selected_boxes = []

        for i in range(self.n, 0, -1):
            if self.dp[i][result_weight][result_boxes] != self.dp[i - 1][result_weight][result_boxes]:
                selected_boxes.append(i)
                result_weight -= int(self.weights[i - 1] * self.scale_factor)
                result_boxes -= 1

        selected_boxes.reverse()

        return max_profit, selected_boxes
```
```python
weights = [2, 5, 4, 3, 6, 1, 4.5]
prices = [200, 700, 600, 400, 900, 150, 500]
max_weight = 15
max_boxes = 5

optimizer = BoxOptimizer(weights, prices, max_weight, max_boxes)
max_profit, selected_boxes = optimizer.solve()

print(f"Maximum Profit: {max_profit}")
print(f"Selected Boxes: {selected_boxes}")
```
```
Maximum Profit: 2200
Selected Boxes: [2, 3, 5]
```
## Course Project for Operational Research
- **Course**: Operational Research [ECE 145]
- **Semester**: Fall 2023
- **Institution:** [School of Electrical & Computer Engineering](https://ece.ut.ac.ir/en/), [College of Engineering](https://eng.ut.ac.ir/en), [University of Tehran](https://ut.ac.ir/en)
- **Instructor:** Dr. Ramezani Moghaddam
