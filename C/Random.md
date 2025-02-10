#### 주의! 랜덤 함수 사용할때 항상 난수 초기화를 해 주어야 한다!
```c
    
//    srand(time(NULL)); //난수 초기화
//    int num = rand() % 3; //3개으 경우의 수 중 하나 (0 ~ 2)
printf("난수 초기화 이전 .. \n");
for (int i = 0; i < 10; i++) {
	printf("%d ", rand() % 10);
}

srand(time(NULL));
printf("\n\n난수 초기화 이후 .. \n");
for (int i = 0; i < 10; i++) {
	printf("%d ", rand() % 10);
}

```