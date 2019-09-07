# FiveInOneLine-with-AI

在线游戏Demo：[https://shenqb.github.io/FiveInALine-with-AI/index.html](https://shenqb.github.io/FiveInALine-with-AI/index.html)

### AI算法基本思路：
* 怎么决定计算机在哪个地方落子？
遍历整个棋盘上还没落子的交叉点，基于AI算法给交叉点计算得分，得分最高的交叉点，即为计算机需要落子的点。

### 难点解析：
### 赢法数组：
* 记录五子棋所有的赢法，三维数组；
* 赢法数组winMethods[i][j][k]前面两个维度i、j为棋盘落子的横纵坐标，第三个维度k表示第k种赢法。
构造赢法数组，共四种可能：
纵向的五子相连，表示一种赢法；
横向的五子相连，表示一种赢法；
左斜向的五子相连，表示一种赢法；
右斜向的五子相连，表示一种赢法；

### 赢法统计数组：
* 记录双方在572种赢法下的落子情况，一维数组；
* 比如humanChessCount[i]表示人类方在第i种赢法下的落子情况,连一子则加1。当humanChessCount[i]=3时表示人类在第i种赢法下连了3颗落子，humanChessCount[i]=5则胜利。

### 如何判断胜负：
* 遍历双方的赢法统计数组，有一方赢法统计数组存在值为5时，则那一方胜利。

### 计算机落子规则：
* 遍历棋局的所有空白点，统计双方在这些位置点上的所有种赢法下的汇总分数，取对人类得分最小或者对计算机得分最大的位置进行落子。核心代码：
```javascript
//计算机每次落子时，最多遍历15*15*527（约13万）次，逐次递减，重新计算当前棋局双方的得分情况：每个空白位置上会有572种赢法，假设每种赢法下双方的得分情况
for(var i = 0; i < 15; i++) {
    for(var j = 0; j< 15;j++) {
        //遍历棋局的所有空白点，统计双方在这些位置点上的所有种赢法下的汇总分数，这一步是计算机落子的依据
        if (chessBoard[i][j] == 0) { 
            for (var k = 0; k < count; k++) {
                if (winMethods[i][j][k]) {
                    //假设这个空白位置点（i,j）是人类第k种赢法下的第1颗子
                    if (humanChessCount[k] == 1) { 
                        humanScore[i][j] += 200
                    //假设这个空白位置点（i,j）是人类第k种赢法下的第2颗子
                    } else if(humanChessCount[k] == 2) { 
                        humanScore[i][j] += 400
                    //假设这个空白位置点（i,j）是人类第k种赢法下的第3颗子
                    } else if(humanChessCount[k] == 3) { 
                        humanScore[i][j] += 2000
                    //假设这个空白位置点（i,j）是人类第k种赢法下的第4颗子
                    } else if(humanChessCount[k] == 4) { 
                                humanScore[i][j] += 10000
                    }

                    if (computerChessCount[k] == 1) {
                        computerScore[i][j] += 220
                    } else if(computerChessCount[k] == 2) {
                        computerScore[i][j] += 420
                    } else if(computerChessCount[k] == 3) {
                        computerScore[i][j] += 2100
                    } else if(computerChessCount[k] == 4) {
                        computerScore[i][j] += 20000
                    }
                }
            }
                    
```
