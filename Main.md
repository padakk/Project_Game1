```java
package rhythm_game_1_16;

public class Main {
	
	public static final int SCREEN_WIDTH = 1280;	//스크린 크기(상수)
	public static final int SCREEN_HEIGHT = 720;	//스크린 크기(상수)
	
	public static final int NOTE_SPEED = 11;		//노트 떨어지는 속도
	public static final int SLEEP_TIME = 10;		//노트가 떨어지는 시간 주기
	public static final int REACH_TIME = 2;			//판정선에 도달하기까지의 시간
	
	public static void main(String[] args) {
			
		new RhythmGame1();
	}
}
```