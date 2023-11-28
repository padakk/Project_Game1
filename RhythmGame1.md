```java
package rhythm_game_1_4;

import java.awt.Color;
import java.awt.Cursor;
import java.awt.Graphics;
import java.awt.Image;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseMotionAdapter;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;

public class RhythmGame1 extends JFrame{//GUI 기반 프로그램을 만들기 위해 필요한 상속
	
	private Image screenImage;
	private Graphics screenGraphic;
	
	//변수에 이미지 초기화
	private Image introBackground = new ImageIcon(Main.class.getResource("../images/introBackground(Title).jpg")).getImage();
	
	private JLabel menuBar = new JLabel(new ImageIcon(Main.class.getResource("../images/menuBar.png")));
	
	//
	private ImageIcon exitButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/exitButtonEntered.png"));
	private ImageIcon exitButtonBasicImage = new ImageIcon(Main.class.getResource("../images/exitButtonBasic.png"));
	private JButton exitButton = new JButton(exitButtonBasicImage);//기본 이미지
	
	private int mouseX, mouseY;					//마우스 좌표
	
	public RhythmGame1() {
		setUndecorated(true); 					//실행했을 때 존재하는 메뉴바가 보이지 않게함
		setTitle("Rhythm Game1");
		setSize(Main.SCREEN_WIDTH, Main.SCREEN_HEIGHT);
		setResizable(false);					//창 크기 변경불가
		setLocationRelativeTo(null); 			//화면 정중앙에 띄우기
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); //게임창을 종료했을 때 프로그램도 같이 종료
		setVisible(true);						//게임창이 화면에 출력되도록 함
		setBackground(new Color(0, 0, 0, 0));	//paintComponents()실행시 배경이 회색이 아닌 하얀색으로 바뀜
		setLayout(null); 						//JLabel 등을 넣었을 때 그 위치 그대로 둔다
		
	
//버튼이 메뉴바보다 먼저 구현되어야 버튼이 메뉴바에 가려지지 않음
		exitButton.setBounds(1245, 0, 30, 30);	//메뉴바의 가장 오른쪽 위치에 버튼위치시키기
		exitButton.setBorderPainted(false);		
		exitButton.setContentAreaFilled(false);
		exitButton.setFocusPainted(false);		//버튼모양바꿔주기
		exitButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {//마우스가 올라갔을 때 버튼이미지 바뀌도록
				exitButton.setIcon(exitButtonEnteredImage);
				exitButton.setCursor(new Cursor(Cursor.HAND_CURSOR));//커서가 손모양으로 바뀌도록
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);//false는 한번만 실행되도록
				buttonEnteredMusic.start();
			}
			@Override
			public void mouseExited(MouseEvent e) {//마우스가 버튼을 벗어났을 때 원래 이미지로
				exitButton.setIcon(exitButtonBasicImage);
				exitButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));//커서가 원래모양으로 돌아오도록
			}
			@Override
			public void mousePressed(MouseEvent e) {//마우스 클릭했을 때
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				try {
					Thread.sleep(1000);				//소리가 먼저 나오고 나중에 종료되도록
				} catch (InterruptedException ex) {
					ex.printStackTrace();
				}
				System.exit(0);						//프로그램 종료
			}
		});
		add(exitButton);
		
				
//메뉴바
		menuBar.setBounds(0, 0, 1280, 30);	
		menuBar.addMouseListener(new MouseAdapter() {
			@Override
			public void mousePressed(MouseEvent e) {//마우스로 버튼을 눌렀을 때
				mouseX = e.getX();					//마우스 좌표 저장
				mouseY = e.getY();
			}
		});
		menuBar.addMouseMotionListener(new MouseMotionAdapter() {
			@Override
			public void mouseDragged(MouseEvent e) {//마우스 드래그 했을 때
				int x = e.getXOnScreen();			//현재 스크린의 마우스좌표 저장
				int y = e.getYOnScreen();	
				setLocation(x - mouseX, y - mouseY);//드래그 할 때 마우스 좌표대로 JFrame위치 바꿔줌
			}
		});
		
		add(menuBar);							//JFrame에 메뉴바 추가
		
			
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
		paintComponents(g);				//JLabel등을 JFrame안에 추가하면 그것을 그려준다.(고정된 이미지)
		this.repaint();					//이미지가 매순간마다 계속 갱신되도록(더블 버퍼링)
	}

}
```