---
published: true
permalink: /algorithm/the-bomberman-game
comments: true
categories:
  - algorithm
title: '[HackerRank] Bomberman Game'
---
HackerRank : The Bomberman Game


## Question
Complete the bomberMan function in the editory below. It should return an array of strings that represent the grid in its final state.

bomberMan has the following parameter(s):

n: an integer, the number of seconds to simulate
grid: an array of strings that represents the grid


## Solution
(1) 첫번째 풀이 (오답) 
- 방법 
  - 짝수일 경우, 전체 String 폭탄 설치 
  - 홀수일 경우, bomberMan(n-2, grid) 호출

- 소스코드 
```c#
 public static string ReplaceAt(string input, int index, char newChar)
    {
        char[] builder = input.ToCharArray();
        builder[index] = newChar;
        return new string(builder);
    }

    public static void detonateBomb(string[] grid) {       
        for (int row = 0; row < grid.Length; row ++)
        {        
            for (int col = 0; col < grid[row].Length; col++)
            {
                if (grid[row][col] == 'O')
                {
                    grid[row] = ReplaceAt(grid[row], col, '.');

                    if (row - 1 >= 0 && grid[row -1][col] == 'x')
                        grid[row - 1] = ReplaceAt(grid[row - 1], col, '.');

                    if (row + 1 <= grid.Length - 1 && grid[row +1][col] == 'x')
                        grid[row + 1] = ReplaceAt(grid[row + 1], col, '.');

                    if (col -1 >= 0 && grid[row][col - 1] == 'x')
                        grid[row] = ReplaceAt(grid[row], col - 1, '.');

                    if (col + 1 <= grid[row].Length -1 && grid[row][col + 1] == 'x')
                        grid[row] = ReplaceAt(grid[row], col + 1, '.');
                }
            }
        }
    }

    public static void plantBomb(string[] grid) {
        for (int row = 0; row < grid.Length; row ++)
        {
            grid[row] = grid[row].Replace('.', 'x');
        }
    }

    public static string[] plantBombAll(string[] grid) {
        for (int row = 0; row < grid.Length; row ++)
        {
            grid[row] = grid[row].Replace('.', 'O');
        }
        return grid;
    }

    public static void cleanBomb(string[] grid) {
        for (int row = 0; row < grid.Length; row ++)
        {
            grid[row] = grid[row].Replace('x', 'O');
        }
    }

    // Complete the bomberMan function below.
    static string[] bomberMan(int n, string[] grid) {
        if (n % 2 == 0)
            return plantBombAll(grid); 

        if (n == 1)
            return grid;       

        plantBomb(grid);
        detonateBomb(grid);    
        cleanBomb(grid);

        return bomberMan(n - 2, grid);
    }
```

(2) 두번째 풀이 (정답) 
- 방법 
  - 짝수일 경우, 전체 String 폭탄 설치 
  - 홀수일 경우, 3,7,11회차는 
    - 3,7,11회차는  `detonateBomb(grid);` 1회 call
    - 5,9,13회차는  `detonateBomb(grid);` 2회 call

- 결과  
  - `Terminated due to timeout`

- 소스코드 
```c#
 private static string ReplaceAt(string input, int index, char newChar)
    {
        char[] builder = input.ToCharArray();
        builder[index] = newChar;
        return new string(builder);
    }

    private static void explodeBomb(string[] grid) {       
        for (int row = 0; row < grid.Length; row ++)
        {        
            for (int col = 0; col < grid[row].Length; col++)
            {
                if (grid[row][col] == 'O')
                {
                    grid[row] = ReplaceAt(grid[row], col, '.');

                    if (row - 1 >= 0 && grid[row -1][col] == 'x')
                        grid[row - 1] = ReplaceAt(grid[row - 1], col, '.');

                    if (row + 1 <= grid.Length - 1 && grid[row +1][col] == 'x')
                        grid[row + 1] = ReplaceAt(grid[row + 1], col, '.');

                    if (col -1 >= 0 && grid[row][col - 1] == 'x')
                        grid[row] = ReplaceAt(grid[row], col - 1, '.');

                    if (col + 1 <= grid[row].Length -1 && grid[row][col + 1] == 'x')
                        grid[row] = ReplaceAt(grid[row], col + 1, '.');
                }
            }
        }
    }

    private static void plantBomb(string[] grid) {
        for (int row = 0; row < grid.Length; row ++)
        {
            grid[row] = grid[row].Replace('.', 'x');
        }
    }

    public static string[] plantBombAll(string[] grid) {
        for (int row = 0; row < grid.Length; row ++)
        {
            grid[row] = grid[row].Replace('.', 'O');
        }
        return grid;
    }

    public static void detonateBomb(string[] grid)
    {
        plantBomb(grid);
        explodeBomb(grid);    
        cleanBomb(grid);
    }


    public static void cleanBomb(string[] grid) {
        for (int row = 0; row < grid.Length; row ++)
        {
            grid[row] = grid[row].Replace('x', 'O');
        }
    }


    // Complete the bomberMan function below.
    static string[] bomberMan(int n, string[] grid) {
        if (n % 2 == 0)
            return plantBombAll(grid); 

        else {
            if (n == 1)
            {
                return grid; 
            }
            else if (n % 4 == 3)
            {
                detonateBomb(grid);
            }
            else if (n % 4 == 1)
            {
                detonateBomb(grid);
                detonateBomb(grid);
            }
        }
        return grid;
    }
```

## Learning... 
- c#의 경우, string의 N번째 index를 가져오려면 `char[]` 혹은 `stringbuilder`를 활용해야하는 것을 배웠다. 
    - [링크](https://soo-toance.github.io/csharp/string-replaceat)


## Reference
- [HackerRank](https://www.hackerrank.com/challenges/bomber-man/problem)
