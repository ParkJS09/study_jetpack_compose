# 슬롯API

- Compose는 UI를 빌드하는 데 사용할 수 있는 상위 수준 [머티리얼 구성요소](https://material.io/components/) 컴포저블을 제공.
    - 이는 UI를 만드는 기본 요소이므로 개발자는 여전히 화면에 표시할 항목에 관한 정보를 제공해야 합니다.
- **슬롯 API**는 컴포저블(이 사용 사례에서는 사용 가능한 머티리얼 구성요소 컴포저블) 위에 맞춤설정 레이어를 가져오려고 Compose에서 도입한 패턴.
- 버튼의 예
    
    `Button(text = "Button")`
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/b3cb99320ec18268.png?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/b3cb99320ec18268.png?hl=ko)
    
    - 해당 버튼의 디자인 변경
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/ef5893f332864e28.png?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/ef5893f332864e28.png?hl=ko)
    
    ```kotlin
    Button(
        text = "Button",
        icon: Icon? = myIcon,
        textStyle = TextStyle(...),
        spacingBetweenIconAndText = 4.dp,
        ...
    )
    ```
    
    - Google에서는 개발자가 맞춤설정 할 수 있는 각 개별 요소의 매개변수를 시도하고 추가할 수 있지만 개발자의 속도를 따라잡기 쉽지 않기에 **예상할 수 없는 방식으로 구성요소를 맞춤설정할 매개변수를 여러 개 추가하는 대신** Google에서는 슬롯을 추가.
    - **슬롯은 개발자가 원하는 대로 채울 수 있도록 UI에 빈 공간을 남겨둡니다.**
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/fccfb817afa8876e.png?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/fccfb817afa8876e.png?hl=ko)
    
    - 버튼의 경우 버튼의 내부를 개발자가 채우도록 남겨둘 수 있습니다. 개발자는 아이콘과 텍스트가 있는 행을 삽입하려고 할 수 있습니다.
    
    ```kotlin
    Button {
        Row {
            MyImage()
            Spacer(4.dp)
            Text("Button")
        }
    }
    ```
    
    - 이를 위해 Google에서는 하위 컴포저블 람다(`content: @Composable () -> Unit`
    )를 사용하는 버튼용 API를 제공합니다.
    - 이를 통해 버튼 내에서 내보내도록 자체 컴포저블을 정의할 수 있습니다
    
    - Button
    
    ```kotlin
    @Composable
    fun Button(
        modifier: Modifier = Modifier,
        onClick: (() -> Unit)? = null,
        ...
        content: @Composable () -> Unit
    )
    ```
    
    - Row
    
    ```kotlin
    @Composable
    inline fun Row(
        modifier: Modifier = Modifier,
        horizontalArrangement: Arrangement.Horizontal = Arrangement.Start,
        verticalAlignment: Alignment.Vertical = Alignment.Top,
        content: @Composable RowScope.() -> Unit
    )
    ```
    
    - Column
    
    ```kotlin
    inline fun Column(
        modifier: Modifier = Modifier,
        verticalArrangement: Arrangement.Vertical = Arrangement.Top,
        horizontalAlignment: Alignment.Horizontal = Alignment.Start,
        content: @Composable ColumnScope.() -> Unit
    )
    ```
    
    - 이름이 `content`인 이 람다는 마지막 매개변수입니다. 이를 통해 **[후행 람다 문법](https://kotlinlang.org/docs/reference/lambdas.html#passing-a-lambda-to-the-last-parameter)**
    을 사용하여 콘텐츠를 구조화된 방식으로 해당 컴포저블에 삽입할 수 있습니다.
    
    - Compose는 상단 앱 바와 같은 좀 더 복잡한 구성요소에서 슬롯을 많이 사용합니다
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/4365ce9b02ec2805.png?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/4365ce9b02ec2805.png?hl=ko)
    
    - 제목을 제외하고 많은 항목을 맞춤설정할 수 있습니다.
    
    ![https://developer.android.com/codelabs/jetpack-compose-layouts/img/2decc9ec64c79a84.png?hl=ko](https://developer.android.com/codelabs/jetpack-compose-layouts/img/2decc9ec64c79a84.png?hl=ko)
    
    ```kotlin
    TopAppBar(
        title = {
            Text(text = "Page title", maxLines = 2)
        },
        navigationIcon = {
            Icon(myNavIcon)
        }
    )
    ```
    
    - 자체 컴포저블을 빌드할 때는 **슬롯 API 패턴을 사용**하면 좀 더 재사용성을 높일 수 있습니다
