# 디자인패턴 - 메멘토 패턴
## 메멘토 패턴이란?
> 객채 내부의 상태를 외부에 저장을 하고 저장된 상태를 다시 복원하고자 할 때에 사용하는 패턴

메멘토패턴을 사용함으로써 객체의 모든 정보를 외부로 노출시키지 않고 캡슐화를 지킬수 있

121
- Originator: 본래의 상태를 가지고 있고 상태를 저장하고 복원하고 싶은 객체
- Memento: immutable 한 객체로 일정 지섬의 Originator 내부정보를 가조기 있음
- CareTaker: Originator 의 내부정보를 Memento type 으로 가지고 있고 복원을 할 수 있는 객체

## 메멘토를 사용하지 않는 코드
가정: 게임의 현재 스테이지와 캐릭터의 정보를 가지고 있는 클래스가 있음
```java
public class Game {

  private int stage;
  private Character character;
  
  public int getStage() {
    return stage;
  }
  
  public void setStage(int stage) {
    this.stage = stage;
  }
  
  public Character getCharacter() {
    return character;
  }
  
  public void setCharacter(Character character) {
    this.character = character;
  }
}
```
게임이 진행 중에 이전 시점으로 돌리는 상활을 가정한 클라이언트 보면
```java
public class Client {
  public static void main(String[] args) {
    Game game = new Game();
    game.setStage(10);
    game.setCharacter(new Character());
    
    int stage = game.getStage();
    Character character = game.getCharacter();
    
    Game restoredGame = new Game();
    restoredGame.setStage(stage);
    restoredGame.setCharacter(character);
  }
}
```

위 코드는 다음 문제점들이 존재
- 캡슐화가 깨지게 된다.
  - 클라이언트가 게임이 구성하는 요소가 무엇인지 알게 됨
- 클라이언트가 Game객체에 의존하게 된다.
  - Game의 맴버가 바뀌면 클라이언트에 코드도 바꿔야함

따라서 메멘토 적용이 필요

## 메멘토패턴 적용해보기
먼저, 메멘토 클래스를 생성
```java
public final class GameSave {

  private final int stage;
  private final Character character;
  
  public GameSave(int stage, Character character) {
    this.stage = stage;
    this.character = character;
  }
  
  public int getStage() {
    return stage;
  }

  public Character getCharacter() {
    return character;
  }
}
```

그리고 Originator인 Game 클래스에서는 메멘토를 사용하여 저장하고 복원할 수 있는 메서드를 만듬
```java
public class Game {

  private int stage;
  private Character character;
  
  public int getStage() {
    return stage;
  }
  
  public void setStage(int stage) {
    this.stage = stage;
  }
  
  public Character getCharacter() {
    return character;
  }
  
  public void setCharacter(Character character) {
    this.character = character;
  }
  
  public GameSave save() {
    return new GameSave(this.stage, this.character);
  }
  
  public Game restore(GameSave gameSave) {
    this.stage = gameSave.getStage();
    this.character = gameSave.getCharacter();
  }
}
```
이렇게 구성을 한다면 클라이언트는 다음과 같아짐
```java
public class Client {

  public static void main(String[] args) {
    Game game = new Game();
    game.setStage(10);
    game.setCharacter(new Character(1, "memento"));

    GameSave save = game.save();

    game.setStage(12);
    game.setCharacter((new Character(15, "visitor")));

    game.restore(save);

    System.out.println(game.getStage());
    System.out.println(game.getCharacter().getLevel());
    System.out.println(game.getCharacter().getName());
  }
}
```

## Reference & Additional Resources
- https://straw961030.tistory.com/362
