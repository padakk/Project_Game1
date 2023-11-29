```java
package rhythm_game_1_11;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics2D;
import java.awt.Image;
import java.awt.RenderingHints;

import javax.swing.ImageIcon;

public class Game extends Thread {//Thread > 프로그램 안에서 실행되는 작은 프로그램

//기본 이미지	
	private Image noteBasicImage = new ImageIcon(Main.class.getResource("../images/noteBasic.png")).getImage();
	private Image noteRouteLineImage = new ImageIcon(Main.class.getResource("../images/noteRouteLine.png")).getImage();
	private Image judgementLineImage = new ImageIcon(Main.class.getResource("../images/judgementLine.png")).getImage();
	private Image gameInfoImage = new ImageIcon(Main.class.getResource("../images/gameInfo.png")).getImage();

	private Image noteRouteSImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteDImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteKImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteLImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	
	public void screenDraw(Graphics2D g) {
		g.drawImage(noteRouteSImage, 430, 30, null);
		g.drawImage(noteRouteDImage, 534, 30, null);
		g.drawImage(noteRouteKImage, 638, 30, null);
		g.drawImage(noteRouteLImage, 742, 30, null);
		g.drawImage(noteRouteLineImage, 426, 30, null);
		g.drawImage(noteRouteLineImage, 530, 30, null);
		g.drawImage(noteRouteLineImage, 634, 30, null);
		g.drawImage(noteRouteLineImage, 738, 30, null);
		g.drawImage(noteRouteLineImage, 842, 30, null);
		g.drawImage(gameInfoImage, 0, 660, null);
		g.drawImage(judgementLineImage, 0, 580, null);
		
		g.drawImage(noteBasicImage, 430, 120, null);
		g.drawImage(noteBasicImage, 534, 400, null);
		g.drawImage(noteBasicImage, 638, 300, null);
		
		//안티앨리어싱(글씨깨짐방지)
		g.setRenderingHint(RenderingHints.KEY_TEXT_ANTIALIASING, RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
		
		//gameInfo 글씨			
		g.setColor(Color.white);
		g.setFont(new Font("굴림", Font.BOLD, 30));
		g.drawString("KUWAGO - Parade", 20, 702);
		g.drawString("Easy", 1190, 702);
		
		g.setFont(new Font("Arial", Font.BOLD, 26));
		g.setColor(Color.DARK_GRAY);
		g.drawString("S", 470, 609);
		g.drawString("D", 574, 609);
		g.drawString("K", 678, 609);
		g.drawString("L", 782, 609);
			
		g.setFont(new Font("Elephant", Font.BOLD, 30));
		g.setColor(Color.white);
		g.drawString("0000000", 578, 702);
	}
//버튼 눌렀을 때 (이펙트, 효과음)
	public void pressS() {
		noteRouteSImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
		new Music("808 CH.mp3", false).start();//효과음
	}
	public void pressD() {
		noteRouteDImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
		new Music("808 CH.mp3", false).start();
	}
	public void pressK() {
		noteRouteKImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
		new Music("808 CH.mp3", false).start();
	}
	public void pressL() {
		noteRouteLImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
		new Music("808 CH.mp3", false).start();
	}
//버튼 뗐을 때	
	public void releaseS() {
		noteRouteSImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	}
	public void releaseD() {
		noteRouteDImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	}
	public void releaseK() {
		noteRouteKImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	}
	public void releaseL() {
		noteRouteLImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	}
		
	@Override
	public void run() {
		
	}
}
```