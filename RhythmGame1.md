```java
package rhythm_game_1_3;

import java.awt.Graphics;
import java.awt.Image;

import javax.swing.ImageIcon;
import javax.swing.JFrame;

public class RhythmGame1 extends JFrame{//GUI 기반 프로그램을 만들기 위해 필요한 상속
	
	private Image screenImage;
	private Graphics screenGraphic;
	
	private Image introBackground;
	
	
	public RhythmGame1() {
		setTitle("Rhythm Game1");
		setSize(Main.SCREEN_WIDTH, Main.SCREEN_HEIGHT);
		setResizable(false);			//창 크기 변경불가
		setLocationRelativeTo(null); 	//화면 정중앙에 띄우기
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); //게임창을 종료했을 때 프로그램도 같이 종료
		setVisible(true);				//게임창이 화면에 출력되도록 함
		
		introBackground = new ImageIcon(Main.class.getResource("../images/introBackground(Title).jpg")).getImage();
		//변수에 이미지 초기화
		
		Music introMusic = new Music("introMusic.mp3", true);
		introMusic.start();
	}
	
	public void paint(Graphics g) {
		screenImage = createImage(Main.SCREEN_WIDTH, Main.SCREEN_HEIGHT);
		screenGraphic = screenImage.getGraphics();
		screenDraw(screenGraphic);
		g.drawImage(screenImage, 0, 0, null);
	}
	
	public void screenDraw(Graphics g) {
		g.drawImage(introBackground, 0, 0, null);
		this.repaint();					//이미지가 매순간마다 계속 갱신되도록(더블 버퍼링)
	}
	
	
}
```