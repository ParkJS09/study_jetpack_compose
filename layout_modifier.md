수정자를 사용하면 컴포저블을 장식하거나 강화할 수 있습니다. 

- 수정자의 기능 :
    - 컴포저블의 크기, 레이아웃, 동작 및 모양 변경
    - 접근성 라벨과 같은 정보 추가
    - 사용자 입력 처리
    - 요소를 클릭 가능, 스크롤 가능, 드래그 가능 또는 확대/축소 가능하게 만드는 것과 같은 높은 수준의 상호작용 추가
- 수정자는 일반 Kotlin 객체
    - 변수에 할당하고 재사용 가능.
    - 여러 수정자를 차례로 체이닝하여 구성 가능.
    
- TODO

![https://developer.android.com/codelabs/jetpack-compose-layouts/img/d2c39f3c2416c321.png?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/d2c39f3c2416c321.png?hl=ko)

- Column과 Text를 활용한 텍스트 2개 노출하기.
    
    ```kotlin
    @Composable
    fun PhotographerCard() {
        Column {
            Text("Alfred Sisley", fontWeight = FontWeight.Bold)
            CompositionLocalProvider(LocalContentAlpha provides ContentAlpha.medium) {
                Text("3 minutes ago", style = MaterialTheme.typography.body2)
            }
        }
    }
    
    @Preview
    @Composable
    fun PhotographerCardPreview() {
        LayoutsCodelabTheme {
            PhotographerCard()
        }
    }
    ```
    
  <img src="https://user-images.githubusercontent.com/46394541/175073701-d1395ee4-82a1-42f5-ac43-eff0e2b4b067.png"/>
    
- Row와 Surface를 활용한 가로로 컴포저블 배치하기
    - Row를 활용하여 가로로 배치가 가능하도록 수정
    - Surface를 활용하여 이미지 노출 영역 추가
        - shape를 활용하여 원형 그리기
        - 사이즈 수정자를 통하여 크기 지정
    
    ```kotlin
    @Composable
    fun PhotographerCard() {
        Row {
            Surface(
                modifier = Modifier.size(50.dp),
                shape = CircleShape,
                color = MaterialTheme.colors.onSurface.copy(alpha = 0.2f)
            ) {
    
            }
            Column {
                Text("Alfred Sisley", fontWeight = FontWeight.Bold)
                CompositionLocalProvider(LocalContentAlpha provides ContentAlpha.medium) {
                    Text("3 minutes ago", style = MaterialTheme.typography.body2)
                }
            }
        }
    }
    ```
    
    <img src ="https://user-images.githubusercontent.com/46394541/175073714-f0f5d5c4-bdfe-470f-9a99-b5f013f09018.png"/>
    
- 컴포저블간의 간격 주기
    - 텍스트가 포함된 Column 영역에 Modifier.padding을 활용하여 컴포저블의 start에 공간을 추가
    
    ```kotlin
    Column(
        modifier = Modifier
            .padding(start = 8.dp)
    ){
    Text("Alfred Sisley", fontWeight = FontWeight.Bold)
        // LocalContentAlpha is defining opacity level of its children
    CompositionLocalProvider(LocalContentAlphaprovides ContentAlpha.medium){
    Text("3 minutes ago", style = MaterialTheme.typography.body2)
    }
    }
    ```
    
    <img src="https://user-images.githubusercontent.com/46394541/175073720-8028a27d-36c9-4d32-98e4-7aa73f1b49c3.png"/>
    
- Column 내의 텍스트 정렬
    - 일부 레이아웃에서는 텍스트와 그 레이아웃 특성에만 적용되는 수정자를 제공
    - `Row`의 컴포저블은 `weight` 또는 `align`과 같이 적절한 특정 수정자에 액세스(Row 콘텐츠의 `RowScope` 수신기에서)할 수 있습니다.
    - 이 범위 지정은 유형 안전성을 제공하므로 다른 레이아웃에 적절하지 않은 수정자를 실수로 사용할 수 없습니다.
    - 예를 들어 `weight`는 `Box`에 적절하지 않으므로 이는 컴파일 시간 오류로 방지됩니다.
    
    ```kotlin
    @Composable
    fun PhotographerCard() {
    Row{
    Surface(
                modifier = Modifier.size(50.dp),
                shape =CircleShape,
                color = MaterialTheme.colors.onSurface.copy(alpha = 0.2f)
            ){
    
            }
    Column(
                modifier = Modifier
                    .padding(start = 10.dp)
                    .align(Alignment.CenterVertically)
            ){
    Text("Alfred Sisley", fontWeight = FontWeight.Bold)
                // LocalContentAlpha is defining opacity level of its children
    CompositionLocalProvider(LocalContentAlphaprovides ContentAlpha.medium){
    Text("3 minutes ago", style = MaterialTheme.typography.body2)
    }
            }
        }
    }
    ```
    
    <img src ="https://user-images.githubusercontent.com/46394541/175073723-1c9cdfdc-279a-443a-b394-93e11111f3be.png"/>
    
    - 대부분의 컴포저블은 선택적인 수정자 매개변수를 허용하여 유연성을 높이므로 호출자가 수정할 수 있습니다.
    - 자체 컴포저블을 만든다면 수정자를 매개변수로 사용하는 것이 좋고 기본값을 `Modifier`
    (아무것도 하지 않는 빈 수정자)로 설정하여 함수의 루트 컴포저블에 적용합니다.
    
    ```kotlin
    @Composable
    fun PhotographerCard(modifier: Modifier = Modifier) {
        Row(modifier) { ... }
    }
    ```
    
    - **참고:**
    규칙에 따라 수정자는 함수의 *첫 번째* 선택적 매개변수로 지정됩니다. 
    이를 통해 개발자는 모든 매개변수의 이름을 지정하지 않아도 컴포저블에서 수정자를 지정할 수 있습니다.
- 수정자 체이닝
    - 수정자 체이닝의 코드에서 팩토리 확장 함수
    (`Modifier.padding(start =8.dp).align(Alignment.CenterVertically)`)를 사용하여 여러 수정자를 차례로 체이닝
    - 순서가 중요하므로 신중하게 수정자를 체이닝해야 합니다.
    - 단일 인수로 연결되므로 순서는 최종 결과에 영향을 미칩니다.
    
    ```kotlin
    @Composable
    fun PhotographerCard(modifier: Modifier = Modifier) {
        Row(modifier
            .padding(16.dp)
            .clickable(onClick = { /* Ignoring onClick */ })
        ) {
            ...
        }
    }
    ```
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/c15a1050b051617f.gif?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/c15a1050b051617f.gif?hl=ko)
    
    - 위의 수정자 체이닝의 결과로는 해당 컴포저블에 전체 영역을 클릭할 수는 없다.
    `padding`이 `clickable` 수정자 *앞에*적용되었기 때문.
    - `padding` 수정자를 `clickable` 수정자 *뒤에*적용하면 패딩이 클릭 가능한 영역에 포함됩니다.
    
    ```kotlin
    @Composable
    fun PhotographerCard(modifier: Modifier = Modifier) {
        Row(modifier
            .clickable(onClick = { /* Ignoring onClick */ })
            .padding(16.dp)
        ) {
            ...
        }
    }
    ```
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/a1ea4c8e16d61ffa.gif?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/a1ea4c8e16d61ffa.gif?hl=ko)
    
- 수정자를 사용하면 컴포저블을 매우 유연하게 수정할 수 있습니다.
    - 외부 간격을 추가하고 컴포저블의 배경 색상을 변경하고 `Row`의 모서리를 둥글게 처리하기
    
    ```kotlin
    @Composable
    fun PhotographerCard(modifier: Modifier = Modifier) {
        Row(modifier
            .padding(8.dp)
            .clip(RoundedCornerShape(4.dp))
            .background(MaterialTheme.colors.surface)
            .clickable(onClick = { /* Ignoring onClick */ })
            .padding(16.dp)
        ) {
            ...
        }
    }
    ```
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/4c7652fc71ccf8dc.gif?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/4c7652fc71ccf8dc.gif?hl=ko)
