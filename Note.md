```java
package rhythm_game_1_16;

import java.awt.Graphics2D;
import java.awt.Image;

import javax.swing.ImageIcon;

public class Note extends Thread {//노트 애니메이션

	private Image noteBasicImage = new ImageIcon(Main.class.getResource("../images/noteBasic.png")).getImage();
	private int x, y = 580 - (1000 / Main.SLEEP_TIME * Main.NOTE_SPEED) * Main.REACH_TIME;//판정선 연산식
	private String noteType;
	private boolean proceeded = true;	//현재 노트의 진행여부
	
	public String getNoteType() {
		return noteType;
	}
	
	public boolean isProceeded() {
		return proceeded;
	}
	
	public void close() {
		proceeded = false;
	}

//생성자
	public Note(String noteType) {
		if(noteType.equals("S")) {
			x = 430;
		}
		else if(noteType.equals("D")) {
			x = 534;
		}
		else if(noteType.equals("K")) {
			x = 638;
		}
		else if(noteType.equals("L")) {
			x = 742;
		}
		
		this.noteType = noteType;
	}
	
	public void screenDraw(Graphics2D g) {//노트 하나씩 그려주기
		if(!(noteType.equals("Space"))) 
		{
			g.drawImage(noteBasicImage, x, y, null);
		}
		else
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
				if(proceeded) {
					Thread.sleep(Main.SLEEP_TIME);//노트 떨어지는 주기
				}
				else {
					interrupt();//스레드 정지
					break;
				}
			}
		} catch (Exception e) {
			System.err.println(e.getMessage());
		}
	}
//노트 판정	
	public void judge() {
		if(y > 620) {
			System.out.println("Miss");
			close();
		}
		else if(y >= 613) {
			System.out.println("Late");
			close();
		}
		else if(y >= 600) {
			System.out.println("Good");
		}
		else if(y >= 587) {
			System.out.println("Great");
		}
		else if(y >= 573) {
			System.out.println("Perfect");
		}
		else if(y >= 565) {
			System.out.println("Great");
		}
		else if(y >= 550) {
			System.out.println("Good");
		}
		else if(y >= 535) {
			System.out.println("Early");
		}
		
	}
}
```