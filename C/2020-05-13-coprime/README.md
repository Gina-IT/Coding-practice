## 주어진 범위 내에 존재하는 서로소 갯수 구하기


### 문제

*" 범위를 입력받고, 해당 범위에 존재하는 서로소 쌍의 개수를 구하는 프로그램 작성  
이때 (A, B)와 (B, A)는 같은 것으로 간주한다. "*  

**참고**  
▶ 서로소? 임의의 두 정수에 대해, 두 수의 공약수가 1만 존재하는 수  

두가지 방법으로 코드를 풀어 실행 시간도 비교해보았다.  
서로소를 구하는 부분의 코드만을 비교하기 위해 범위를 입력받고 출력하는   
main함수 부분은 동일하게 작성하고  서로소를 구하는 코드 coprime을 달리하여 비교한다.

----------

**< main 함수 > - 범위 입력받기, 실행, 출력**
```c
int main() {
   	int a, b;
   
   	printf("서로소를 구할 범위를 입력하세요.\n");
   	printf("시작: ");
   	scanf("%d", &a);
   	printf("끝: ");
   	scanf("%d", &b);
      
   	printf("\n%d부터 %d까지 서로소: \n", a,b);

	coprime(a, b);
	
   	printf("\n");
   	printf("\n서로소 갯수: %d개\n", cnt);
}
```

----------

### Algorithm 1.  

2부터 (범위의 끝 값 /2)까지 모든 수 중에  
두 정수 A, B가 동시에 나누어 떨어지는 수가 있다면 count하지 않고  
두 정수 A, B가 동시에 나누어 떨어지는 수가 없다면 count++   

```c
int coprime(int start, int end){

	int A, B;
	int idx, ex;
	int cnt=0;
	
	for( A= start; A < end; A++ ){
		for( B= A+1; B <= end; B++ ){          // (A, B)와 (B, A)가 동시에 나오지 않도록 B의 범위를 A보다 큰 값으로 잡음 
			for( idx = 2 ; idx <= end/2 ; idx++ ){      
				if( A % idx == 0 && B % idx == 0){     // A와 B가 모두 idx로 나누어 지면 idx는 A와 B의 공약수 
					ex=0;     // 1이 아닌 공약수(idx)가 존재하므로 서로소가 아님-> ex값 0반환 
					break;     // 반복문 벗어남  
				}
				else ex=1;     // 공약수가 아님-> ex값 1반환 
				// 반복문을 벗어나지 않고 idx = end/2까지 실행했다면
				// 1외에 A와 B의 공약수는 없는 것이기 때문에 두 수는 서로소이다. 
			}
			
			// if()에 0이 아닌 수가 들어가면 참으로 읽혀 if문 수행함 (0이면 if문 수행하지 않고 넘어감) 
			if (ex){     // ex에 1이 들어가면, 즉 서로소이면 
				printf("(%d, %d)  ", A, B);
				cnt++;
			}
		}
	}
} 
```

### Result  

범위를 10부터 20으로 주었을 때  
<img src= "/C/2020-05-13-coprime/_img/coprime_result.jpg">  

----------

▶  유클리드 호제법: 두 수의 최대공약수를 구하는 알고리즘  
<img src="/C/2020-05-13-coprime/_img/gcd.jpg">  

```
유클리드 호제법 예시 (나머지 연산 이용)  

x=252, y=105   
① z = x % y = 252 % 105 = 42(결과 x=105, y=42)   
② z = x % y = 105 % 42= 21(결과 x=42, y=21)   
③ z = x % y = 42 % 21= 0(결과 x=21, y=0)   
=> 나머지가 0이면 *제수의 값이 최대공약수이다  
    ( *제수: 나눗셈에서 어떤 수를 나누는 수 )  
```

### Algorithm 2.  

'어떤 두 수의 최대공약수가 1이면 두 수는 공약수'임을 이용  
정수 A는 범위의 시작부터 A++,  정수 B는 범위의 끝부터 B--  
최대공약수가 1이면 count++, B가 A보다 작아지면 반복문 종료  

```c
int coprime(int start, int end){

	int A, B;
	int idx, ex;
	
	for( A= start; A < end; A++ ){
		for( B= A+1; B <= end; B++ ){          // (A, B)와 (B, A)가 동시에 나오지 않도록 B의 범위를 A보다 큰 값으로 잡음 
			// 'B % A'를 반복해서 최대공약수 구하기 때문에 'B > A'여야 하므로 큰 값의 범위인 B를 앞에 넣어줌 
			if ( gcd(B, A) == 1 ){    // gcd의 반환값(최대공약수)이 1이면 
				printf("(%d, %d)  ", A, B);
				cnt++;
			}
		}
	}
} 

// 두 수의 최대공약수가 1이면 서로소임을 활용하기 위해
// 유클리드 호제법+재귀함수를 통해 최대공약수 구하기
int gcd(int x, int y) {
	if (y == 0) return x;     // 최대공약수 값 x 반환 
	gcd(y, x%y);
}
```

### Result  

범위를 10부터 20으로 주었을 때  
<img src= "/C/2020-05-13-coprime/_img/coprime2_result.jpg">  
Algorithm 1에 비해 시간이 단축된 것을 확인 가능  

----------

## 실행 시간의 차이 - 효율성 문제     

작은 범위에서 값을 구할 때는 시간의 차이가 크게 없지만,  
구하고자 하는 범위가 커지면 실행 시간에 훨씬 큰 차이가 생기는 것을 확인할 수 있다.  

범위가 커지면 출력되는 서로소가 너무 많기 때문에 서로소를 출력하는 부분은 제외하고 개수만 출력하였다.  

### Algorithm 1  

<img src="/C/2020-05-13-coprime/_img/coprime_algorithm1_result.jpg">  
  
### Algorithm 2   

<img src="/C/2020-05-13-coprime/_img/coprime_algorithm2_result.jpg">  
시간이 20배 이상 줄어든 것을 확인할 수 있다.

