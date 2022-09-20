# k진수에서 소수 개수 구하기 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/92335

## 접근 방식
조건을 읽어보면 결국    
1. n을 k진수화 시킨고
2. 그 값을 0으로 split 한 뒤    
3. 그 중 10진수로 소수인 숫자의 갯수를 구하라

는 문제이다.

## 문제 풀이
```c++
#include <string>
#include <vector>

#include <algorithm>
#include <cmath>

using namespace std;

string ConvertToKNotation(int n, int k)
{
    string str = "";
    
    while (n != 0)
    {
        str += to_string(n % k);
        n /= k;
    }
    
    reverse(str.begin(), str.end());
    
    return str;
}

void SplitByZero(vector<long long>& v, string target)
{
    string str;
    for (int i = 0; i < target.length(); ++i)
    {
        if (target[i] == '0') continue;
        
        string result = "";
        int j;
        for (j = i; j < target.length(); ++j)
        {
            if (target[j] != '0')
            {
                result += target[j];
            }
            else
                break;
        }
        if (result != "1")
            v.push_back(stoll(result));
        
        i = j;
    }
}

int CheckPrime(vector<long long>& v)
{
    int result = 0;
    for (int i = 0; i < v.size(); ++i)
    {
        long long sqrtNum = sqrt(v[i]);
        bool isPrime = true;
        
        for (long long j = 2; j <= sqrtNum; ++j)
        {
            if (v[i] % j == 0)
            {
                isPrime = false;
                break;
            }
        }
        
        if (isPrime)
            result++;
    }
    
    return result;
}

int solution(int n, int k) {
    string target = ConvertToKNotation(n, k);
    
    vector<long long> v;
    
    SplitByZero(v, target);
    
    return CheckPrime(v);
}
```
함수는 간단하게 3단계로 나누었다.
1. n을 k진수 string으로 변환하고
2. 변환된 string을 0으로 split 하여 vector에 저장 (이 때 1은 제외)
3. vector에 저장된 값들 중에 소수가 있는지 판단하여 answer 증가시키기

## 다시 생각해볼 점
기본적으로는 주어진 조건대로 함수만 잘 구현하면 해결되는 문제이다.  
하지만 한가지 오류로 인해 계속 test case 1번과 11번이 오류가 났었다.    
원인은 바로 자료형 문제였다.
처음엔 소수문제의 정석인 에라토스테네스의 체를 이용해서 풀었었다.    
단순하게 n의 최대값인 1'000'000이 최대로 나올 수라고 생각하고 dp로 arr[1'000'000]을 선언했다.   
하지만 n의 최대값을 2진수로 변환해보면 저 최대값의 제곱정도는 되는 수가 나온다.     
이는 int 자료형에 담길 범위도 아니고, 배열로 미리 만들어서 계산할 수 있는 수가 아니다.  
그래서 이를 깨닫고 2번에서 vector에 저장할 자료형을 long long으로 바꾸고, 3번에서 소수판별도 그때그때 sqrt범위까지 체크하는 식으로 바꿨더니 해결되었다.