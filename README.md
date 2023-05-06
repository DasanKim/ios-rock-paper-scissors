# ✊✌✋ 묵찌빠 게임

## 📖 목차
1. [소개](#-소개)
2. [팀원](#-팀원)
3. [타임라인](#-타임라인)
4. [시각화된 프로젝트 구조](#-시각화된-프로젝트-구조)
5. [실행 화면](#-실행-화면)
6. [트러블 슈팅](#-트러블-슈팅)
7. [참고 링크](#-참고-링크)
8. [팀 회고](#-팀-회고)

</br>

## 🍀 소개
민다훈(`MINT`, `DasanKim`, `hoon`) 팀이 만든 묵찌빠 게임입니다. 사용자는 컴퓨터와의 가위바위보를 진행하여 묵찌빠의 선후를 정합니다. 이후 묵찌빠의 게임을 이어서 진행하며 승리자를 출력합니다.

* 주요 개념: `Control Flow`, `Fuctions`, `Enumerations`, `Properties`, `Methods`

</br>

## 👨‍💻 팀원
| MINT | DasanKim | hoon |
| :--------: | :--------: | :--------: |
| <Img src = "https://i.imgur.com/QZAM3wu.jpg" width="200"> |<Img src="https://i.imgur.com/EU67fox.jpg" width="200"> |<Img src="https://i.imgur.com/muRP0gP.png" width="200"> |
|[Github Profile](https://github.com/mint3382) |[Github Profile](https://github.com/DasanKim) |[Github Profile](https://github.com/Hoon94)



</br>

## ⏰ 타임라인
|날짜|내용|
|:--:|--|
|2023.05.01.| - 가위바위보 순서도 작성 <br> - `selectMenu`함수 구현 <br> - `generateComputerRandomNumber`함수 구현 <br> - `comparePick`함수 구현|
|2023.05.02.| - `comparePick` 함수 리팩토링 <br> - PR comment 작성|
|2023.05.03.| - 묵찌빠 순서도 작성 <br> - `Player, REslut, Game` 열거형 구현 <br> - `compareMukJjiPpa, playMukJjiPpa, printReslut` 함수 구현 <br> - `GameManager, RockPaperScissorsGame, MukJjiPpaGame` 클래스 구현| 
|2023.05.04.| - `GameBase class` 생성 <br> - 묵찌빠 class diagram 작성 <br> - class design 수정|
|2023.05.05.| - `GameBase class`를 `Gameable protocol`로 리팩토링 <br> - README 작성|

</br>

## 👀 시각화된 프로젝트 구조

### Flowchart
<p align="center">
<img width="800" src="https://i.imgur.com/PF0LmvA.png">
</p>

### Class Diagram
<p align="center">
<img width="800" src="https://i.imgur.com/EQiH8Yi.jpg">
</p>

</br>

## 💻 실행 화면 

### 가위바위보
| 비겼을 경우 | 사용자 승리 | 컴퓨터 승리 | 잘못된 입력 | 게임 종료 |
|:--:|:--:|:--:|:--:|:--:|
|<img src="https://i.imgur.com/spPCIpO.gif" width="300">|<img src= "https://i.imgur.com/AVTz85C.gif" width="300">|<img src= "https://i.imgur.com/VJKxRKi.gif" width="300">|<img src= "https://i.imgur.com/sN4PZUt.gif" width="300">|<img src= "https://i.imgur.com/XOyjjfH.gif" width="300">|


### 묵찌빠
| 사용자 승리 | 컴퓨터 승리 | 잘못된 입력 | 게임 종료 |
|:------:|:--:|:--:|:--:|
|<img src= "https://i.imgur.com/MvEFjGN.gif" width="300">|<img src= "https://i.imgur.com/hVkF8qQ.gif" width="300">|<img src= "https://i.imgur.com/vPfMURh.gif" width="300">|<img src= "https://i.imgur.com/JZF5P1O.gif" width="300">|

</br>

## 🧨 트러블 슈팅
1️⃣ **재귀와 반복문 사용** <br>
-
🔒 **문제점** <br>

- 처음 일정한 동작을 반복하는 과정에서 재귀를 사용하였습니다. 같은 동작을 반복하는 과정에서 사용할 수 있는 방법으로 반복문과 재귀, 두 가지 방법이 있었습니다. 코드의 간결하게 작성할 수 있는 재귀를 사용하였습니다. 반복문과 재귀의 비교를 확인하였을 때 재귀는 함수 호출 시 스택 메모리 영역에 호출 때마다 쌓여 메모리 영역을 초과할 시 스택오버플로우가 발생할 수 있는 문제점을 알게 되었습니다.

    ``` swift
    guard let pick = readLine(), let menu = Menu(rawValue: pick) else {
        print("잘못된 입력입니다. 다시 시도해주세요.")
        selectMenu()
        return
    }
    ```

🔑 **해결방법** <br>

- `while`문을 사용하여 반복문을 사용하였습니다. 재귀 함수를 사용하는 것처럼 함수를 시작하고서 내부 구현 부분을 전체를 `while`문으로 묶어서 처리하였습니다.

    ``` swift
    func startRockPaperScissors() -> (Player, Result) {
        while matchResult == .draw {
            print("가위(1), 바위(2), 보(3)! <종료 : 0>:", terminator: " ")

            guard let input = readLine(),
                  let index = Int(input),
                  RockPaperScissorsMenu.allCases.indices.contains(index)
            else {
                print("잘못된 입력입니다. 다시 시도해주세요.")
                continue
            }

            playGame(user: index)
            printResult()
        }

        return (turn, matchResult)
    }
    ```
<br>

2️⃣ **여러 값의 비교** <br>
-
🔒 **문제점** <br>

- 매개변수 2개를 받아서 값을 `if-else`문을 사용하여 각각의 값을 비교하는 방법으로 코드를 작성하였습니다. 전달인자로 들어온 값을 각각 사용하여 개별적으로 비교하여 `&&` 연산을 사용하여 조건을 완성하였습니다.

    ```swift
    func comparePick(with user: String, and computer: String) {   
            if (user == "2" && computer == "1")
                || (user == "1" && computer == "3")
                || (user == "3" && computer == "2") {
                print("이겼습니다")
            } else if user == computer {
                print("비겼습니다")
            } else {
                print("졌습니다")
            }
        }
    ```
🔑 **해결방법** <br>

- 두 개의 매개변수를 `tuple`을 사용하여 하나의 매개변수로 전달하고 `tuple`끼리 비교 연산을 사용하여 문제를 해결하였습니다.

    ```swift
    private func comparePick(with pair: (user: String, computer: String)) {
            let winningPair = [("2", "1"), ("1", "3"), ("3", "2")]

            if winningPair.contains(where: {$0 == pair}) {
                print("이겼습니다!")
            } else if pair.user == pair.computer {
                print("비겼습니다!")
            } else {
                print("졌습니다!")
            }
        }
    ```

<br>

3️⃣ **String 비교 대신 Menu 비교** <br>
-
🔒 **문제점** <br>
- 가위바위보의 결과를 비교하는 과정에서 `Menu`의 `rawValue`인 `String` 타입으로 비교하였습니다. 그러니 코드를 보았을 때 어떤 짝인지, 무엇을 비교하는지에 대해서 직관적으로 이해가 되지 않았습니다. 
    ```swift
    private func comparePick(with pair: (user: String, computer: String)) {
            let winningPair = [("2", "1"), ("1", "3"), ("3", "2")]
    ```

🔑 **해결방법** <br>
- `rawValue`를 줄 필요 없이 `Menu`를 받아서 `Menu`를 비교하는 방법으로 변경하였습니다. 가위바위보 Pair를 바로 확인할 수 있어서 어떠한 것들을 비교하는지 더 명확해졌고, `if 문`에서 비교할 때 `rawValue`를 줄 필요도 없어서 타입에 있어서도 직관성이 올라갔습니다. 
    ```swift
    private func compareRockPaperScissors(_ user: RockPaperScissorsMenu, _ computer: RockPaperScissorsMenu) {
            let winningPair: [(user: RockPaperScissorsMenu, computer: RockPaperScissorsMenu)] = [(.rock, .scissors),
                                                                                                 (.scissors, .paper),
                                                                                                 (.paper, .rock)]

            if winningPair.contains(where: { $0.user == user && $0.computer == computer }) {
                matchResult = .win
                turn = .user
            } else if user == computer {
                matchResult = .draw
            } else {
                matchResult = .lose
                turn = .computer
            }
        }
    ```

<br>

4️⃣ **class diagram** <br>
-
🔒 **문제점** <br>
- 순서도만을 가지고 함수들을 만든 후 객체를 나누었더니 SOLID 원칙에도 위배되고 각각의 객체들이 결합도가 높아서 한 가지 기능을 수정하려면 다른 곳들까지 전부 바꿔야 하는 문제가 심했습니다.

🔑 **해결방법** <br>

- 객체 간의 역할과 책임에 대해 고민하면서 class diagram을 그려보았습니다. 각자의 것이 명확하니 함수 하나 당 하나의 일을 하도록 만들 수 있었고 코드의 직관성도 나아졌습니다. 

    ![](https://i.imgur.com/TjLHXX0.jpg)


5️⃣ **`class` 초기화** <br> <br>
-
🔒 **문제점** <br>

- `class`에 선언한 프로퍼티의 초기화를 위해 실제 사용하지 않는 값을 사용하여 초기화를 하였습니다. 초기화 한 후에 실제로 필요한 값을 받아 사용하기 위해 메서드에서 입력으로 사용해야 할 값들을 매개변수로 받아오도록 코드를 작성하였습니다.

    ```swift
    init() {
        rockPaperScissorsGame = RockPaperScissorsGame(turn: turn, matchResult: matchResult, selectedGame: selectedGame)
        mukJjiPpaGame = MukJjiPpaGame(turn: turn, matchResult: matchResult, selectedGame: selectedGame)
    }
    ```

🔑 **해결방법** <br>

- 프로퍼티를 처음 초기화하는 것이 아닌 사용을 할 때 초기화를 진행하기 위해 옵셔널을 사용하여 프로퍼티를 선언하였고 실제 필요한 `owner`와 `result`를 할당받은 후에 초기화를 진행하도록 하였습니다.

    ```swift
    private var mukJjiPpaGame: MukJjiPpaGame?
    mukJjiPpaGame = MukJjiPpaGame(turn: owner, matchResult: result)
    ```


6️⃣ **printResult의 위치** <br>
-
🔒 **문제점** <br>
- 결과를 출력하는 객체가 `GameManager`일지 `RockPaperScissorsGame`과 `MukJjiPpaGame`일지에 대한 의논이 있었습니다. `GameManager`에서 출력하게 된다면 다를 `Game들`의 객체에서 반복하며 출력하는 부분의 처리가 지저분하게 `GameManager`에서 조건을 많이 걸어주어야 했습니다.

    ```swift
    func playGame() {
            (turn, matchResult) = rockPaperScissorsGame.playRockPaperScissors()

            guard let owner = turn, let result = matchResult, result != .giveUp
            else {
                print("게임 종료")
                return
            }

            print("\(result.koreanMessage)")
            mukJjiPpaGame = MukJjiPpaGame(turn: owner, matchResult: result)

            guard let player = mukJjiPpaGame?.playMukJjiPpa(), player.status != .giveUp
            else {
                print("게임 종료")
                return
            }

            print("\(player.winner.koreanMessage)의 승리!")
        }
    }
    ```

🔑 **해결방법** <br>

- `RockPaperScissorsGame`과 `MukJjiPpaGame`가 결과를 출력하게 전환하였습니다. `GameManager`는 단순하게 가위바위보 게임을 시작하게 하고, 가위바위보 결과에 따라 묵찌빠 게임을 시작하게 하며 묵찌빠 게임이 끝나면 그대로 프로그램을 종료시켜주는 역할을 맡게 했습니다.

    ```swift
    //playGame()에서는 Print를 삭제
    func playGame() {
            (turn, matchResult) = rockPaperScissorsGame.startRockPaperScissors()

            guard let owner = turn, let result = matchResult, result != .giveUp else { return }

            mukJjiPpaGame = MukJjiPpaGame(turn: owner, matchResult: result)
            mukJjiPpaGame?.startMukJjiPpa()
        }

    //rockPaperScissorsGame에서의 PrintReslut()
    func printResult() {
            print("\(matchResult.koreanMessage)")
        }

    //MukJjiPpaGame에서의 PrintResult()
    func printResult() {
            switch matchResult {
            case .win, .lose:
                print("\(turn.koreanMessage)의 턴입니다.")
            case .draw:
                print("\(turn.koreanMessage)의 승리!")
            case .giveUp:
                print("게임 종료💀")
            }
        }
    ```

</br>

## 📚 참고 링크
- [🍎Apple Docs: Enumerations](https://www.swift.org/documentation/api-design-guidelines/https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#Initializing-from-a-Raw-Value)
- [🍎Apple Docs: Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes)
- [🍎Apple Docs: Understanding Swift Performance](https://developer.apple.com/videos/play/wwdc2016/416/)
- [🍎Apple Docs: CaseIterable](https://developer.apple.com/documentation/swift/caseiterable)
- [📘stackOverflow: tuples](https://stackoverflow.com/questions/29736244/how-do-i-check-if-an-array-of-tuples-contains-a-particular-one-in-swift)
- [📘stackOverflow: enum](https://stackoverflow.com/questions/40636469/can-i-restrict-an-enum-to-certain-cases-of-another-enum)
- [📘blog: solid](https://soojin.ro/blog/solid-principles-in-swift)
</br>

## 👥 팀 회고
- [팀 회고 링크](https://github.com/mint3382/ios-rock-paper-scissors/blob/main/%ED%8C%80%ED%9A%8C%EA%B3%A0.md)
