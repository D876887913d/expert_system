# 专家系统

- [x] 井字棋：Minimax算法 + Alpha-Beta算法 
    
    具体代码见：src/tic-tac-toe-minimax.py
- [ ] 推理机：正向推理机、反向推理机
- [ ] 决策树：ID3算法

# Minimax算法 + Alpha-Beta算法
```python
    def minimax(self, board, player, next_player, alpha=-2, beta=2):
        # 递归结束条件为其中一方胜利或者二者平局，如果是电脑方胜利的话，那么就返回+1，反之为-1.
        if winner(board, COMPUTER):  # 电脑胜利
            return +1
        if winner(board, HUMAN):  # 人类胜利
            return -1
        elif not empty(board):
            return 0  # 平局

        # ========================极大极小树遍历================================
        for move in range(ROW * COL):
            if board[move] == SPACE:
                board[move] = player
                val = self.minimax(board, next_player, player, alpha, beta)
                board[move] = SPACE  # 重置

                # 更新alpha跟beta的数值
                if player == COMPUTER:  # 极大 max value
                    if val > alpha:
                        alpha = val
                    if alpha >= beta:  # pruning
                        return beta

                else:  # 极小 min value
                    if val < beta:
                        beta = val
                    if beta <= alpha:  # 剪枝
                        return alpha

        # ========================极大极小树遍历结束==============================

        if player == COMPUTER:
            return alpha
        else:
            return beta

    def move(self, board):
        best = -2
        my_moves = []
        for move in range(ROW * COL):
            if board[move] == SPACE:  # 尝试下棋
                board[move] = COMPUTER  # 记录
                score = self.minimax(board, HUMAN, COMPUTER)  # 思考对手怎么下棋
                board[move] = SPACE  # 重置

                if score > best:  # 找到更优的位置
                    best = score
                    my_moves = [move]
                if score == best:  # 一样优秀的位置
                    my_moves.append(move)

        pos = random.choice(my_moves)  # 随机挑出一个位置
        board[pos] = COMPUTER
```

# 推理机

## 正向推理（数据驱动），其基本思想是：
1. 从问题已有的事实（初始证据）出发，正向使用规则，
2. 当规则的条件部分与已有的事实匹配时，就把该规则作为可用规则放入候选规则队列中，
3. 然后通过冲突消解，在候选队列中选择一条规则作为启用规则进行推理，并将其结论放入数据库中，作为下一步推理时的证据。
4. 如此重复这个过程，直到再无可用规则可被选用或者求得了所要求的解为止。

## 冲突消解：
1. 优先度排序。事先给知识库中每条规则设定优先度参数，优先度高的规则先执行。
2. 规则的条件详细度排序。条件较多、较详细的规则，其结论一般更接近于目标，优先执行。
3. 匹配度排序。事先给知识库中每条规则设定匹配度参数，匹配度高的规则先执行。
4. 根据领域问题的特点排序。根据领域知识可以知道的某些特点，事先设定知识库中的规则的使用顺序。

