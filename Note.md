```java
package rhythm_game_1_14;

import java.awt.Graphics2D;
import java.awt.Image;

import javax.swing.ImageIcon;

public class Note extends Thread {//노트 애니메이션

	private Image noteBasicImage = new ImageIcon(Main.class.getResource("../images/noteBasic.png")).getImage();
	private int x, y = 580 - 1000 / Main.SLEEP_TIME * Main.NOTE_SPEED;//판정선 연산식
	private String noteType;
	
	public Note(int x, String noteType) {//노트 좌표
		this.x = x;
		this.noteType = noteType;
	}
	
	public void screenDraw(Graphics2D g) {//노트 하나씩 그려주기
		if(noteType.equals("short")) 
		{
			g.drawImage(noteBasicImage, x, y, null);
		}
		else if(noteType.equals("long")) //스페이스바
		{
			g.drawImage(noteBasicImage, x, y, null);
			g.drawImage(noteBasicImage, x+100, y, null);
		}
	}
	
	public void drop() {//노트 떨어지는 속도
		y += Main.NOTE_SPEED;
	}
	
	@Override
	public void run() {//스레드가 실행되는 함수
		try {
			while (true) 
			{
				drop();
				Thread.sleep(Main.SLEEP_TIME);//노트 떨어지는 주기
			}
		} catch (Exception e) {
			System.err.println(e.getMessage());
		}
	}
}
```