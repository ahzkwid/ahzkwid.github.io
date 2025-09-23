


# 2025-09-24

<img width="1109" height="794" alt="image" src="https://github.com/user-attachments/assets/84f140bf-0fc8-43f9-8f68-c582b4f2cc61" />

아무것도 없을때

<img width="552" height="484" alt="image" src="https://github.com/user-attachments/assets/8cea14f8-8236-4f67-af3f-39c8fc668cf3" />

의자 48개 배치

<img width="1236" height="600" alt="image" src="https://github.com/user-attachments/assets/83052a53-dc1a-475d-b5ea-6998865ce24c" />

프레임이 20% 떨어졌다

<img width="1017" height="496" alt="image" src="https://github.com/user-attachments/assets/2408cca8-3e88-4a5f-b812-e4e280e68c03" />

아무것도 없을때도 동일한것을 보니 렌더링부담은 아니다


```
void Update()
{
    var time = Time.time;
    if (time<lastCheckTime+0.1f)
    {
        return;
    }
    lastCheckTime = time;



    UpdateScaleChair();
    UpdateOffset();
}
void UpdateOffset()
{
    if (seatedPlayer == null)
    {
        return;
    }
}


```


그래서 이렇게 바꿨다

```
void Update()
{
    var time = Time.time;
    if (time<lastCheckTime + 0.1f)
    {
        return;
    }
    lastCheckTime = time;

    //UpdateScaleChair();
    UpdateOffset();
}
void UpdateOffset()
{
    if (seatedPlayer == null)
    {
        return;
    }
}
```


<img width="705" height="505" alt="image" src="https://github.com/user-attachments/assets/a07294d5-de0e-41f2-9f18-52494e459653" />


그랬더니 이렇게 나온다

기존 offset 보정이 y축만 되었기 때문

참고로 프레임 변화는 없었다

UpdateOffset은 앉아있지 않다면 전혀 작동하지 않기 때문에 Update문제가 아니라 VRChat자체의 동기화 부담으로 보인다

그렇다면 VRC Station을 20개 이하로 제한하는것이 좋을것이다


<img width="621" height="782" alt="image" src="https://github.com/user-attachments/assets/2a4795ec-e67c-4bc6-8568-4e8a48f5d2b2" />

<img width="888" height="918" alt="image" src="https://github.com/user-attachments/assets/d42dd33d-7dd8-47aa-ad11-f21b6e59c04d" />

그리고 수정했더니 이렇게 앉고나면 offset이 틀어지는 문제가 있었다


원래있던 문제일것이나 scale보정때문에 몰랐던것이랴 이것도 수정했다


```
void Update()
{
    var time = Time.time;
    if (time > lastCheckTime + 0.1f)
    {
        UpdateOffset();
        lastCheckTime = time;
    }
    UpdatePosition();
}
```

Lerp가 0.1초 이동때문에 매끄럽지 않았다

그래서 Lerp보정부분을 별도 함수로 분리했다


Hip,UpperLeg기준으로 위치를 잡던것에 LowerLeg도 추가함


<img width="1920" height="1080" alt="VRChat_2025-09-24_06-54-33 297_1920x1080" src="https://github.com/user-attachments/assets/0e0e6b91-e095-4320-a003-1f06d311d6ef" />

<img width="1920" height="1080" alt="VRChat_2025-09-24_07-39-50 683_1920x1080" src="https://github.com/user-attachments/assets/2406d0f1-c6a7-459d-b0ba-490d900a37f5" />

완료
