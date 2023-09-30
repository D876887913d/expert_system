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