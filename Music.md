```java
package rhythm_game_1_16;

import java.io.BufferedInputStream;
import java.io.File;
import java.io.FileInputStream;

import javazoom.jl.player.Player;

public class Music extends Thread {	//스레드 = 하나의 작은 프로그램
	
	private Player player;
	private boolean isLoop;			//곡이 무한반복인지 한번만 반복인지
	private File file;				
	private FileInputStream fis;
	private BufferedInputStream bis;
	
	public Music(String name, boolean isLoop) {
		try {
			this.isLoop = isLoop;
			file = new File(Main.class.getResource("../music/" + name).toURI());//toURI() > 해당파일의 위치 가져오기
			fis = new FileInputStream(file);
			bis = new BufferedInputStream(fis);//읽어오기
			player = new Player(bis);
		} catch (Exception e) {		//예외처리(오류 발생 시 catch문 실행)
			System.out.println(e.getMessage());
		}
	}
	
	public int getTime() {	//현재 음악이 실행되는 위치를 알려줌
		if (player == null)
			return 0;
		return player.getPosition();
	}
	
	public void close() { 	//음악을 종료하는 메서드
		isLoop = false;
		player.close();
		this.interrupt(); 	//해당 스레드를 중지상태로 만듬
	}
	
	@Override
	public void run() {
		try {
			do {
				player.play();
				fis = new FileInputStream(file);
				bis = new BufferedInputStream(fis);
				player = new Player(bis);
			} while (isLoop);//isLoop == true이면 무한반복
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```