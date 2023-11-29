```java
package rhythm_game_1_10;

import java.awt.Color;
import java.awt.Cursor;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.Image;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseMotionAdapter;
import java.util.ArrayList;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;

public class RhythmGame1 extends JFrame{//GUI 기반 프로그램을 만들기 위해 필요한 상속
	
	private Image screenImage;
	private Graphics screenGraphic;
	
//종료버튼 이미지
	private ImageIcon exitButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/exitButtonEntered.png"));
	private ImageIcon exitButtonBasicImage = new ImageIcon(Main.class.getResource("../images/exitButtonBasic.png"));	
//시작화면 메뉴버튼 이미지
	private ImageIcon startButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/startButtonEntered.png"));
	private ImageIcon startButtonBasicImage = new ImageIcon(Main.class.getResource("../images/startButtonBasic.png"));
	private ImageIcon quitButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/quitButtonEntered.png"));
	private ImageIcon quitButtonBasicImage = new ImageIcon(Main.class.getResource("../images/quitButtonBasic.png"));
//좌우버튼	
	private ImageIcon leftButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/leftButtonEntered.png"));
	private ImageIcon leftButtonBasicImage = new ImageIcon(Main.class.getResource("../images/leftButtonBasic.png"));
	private ImageIcon rightButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/rightButtonEntered.png"));
	private ImageIcon rightButtonBasicImage = new ImageIcon(Main.class.getResource("../images/rightButtonBasic.png"));	
//난이도버튼	
	private ImageIcon easyButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/easyButtonEntered.png"));
	private ImageIcon easyButtonBasicImage = new ImageIcon(Main.class.getResource("../images/easyButtonBasic.png"));
	private ImageIcon hardButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/hardButtonEntered.png"));
	private ImageIcon hardButtonBasicImage = new ImageIcon(Main.class.getResource("../images/hardButtonBasic.png"));
//뒤로가기버튼
	private ImageIcon backButtonEnteredImage = new ImageIcon(Main.class.getResource("../images/backButtonEntered.png"));
	private ImageIcon backButtonBasicImage = new ImageIcon(Main.class.getResource("../images/backButtonBasic.png"));
	
//이미지
	private Image gameInfoImage = new ImageIcon(Main.class.getResource("../images/gameInfo.png")).getImage();
	private Image judgementLineImage = new ImageIcon(Main.class.getResource("../images/judgementLine.png")).getImage();
	private Image noteRouteImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteLineImage = new ImageIcon(Main.class.getResource("../images/noteRouteLine.png")).getImage();
	
	private Image noteBasicImage = new ImageIcon(Main.class.getResource("../images/noteBasic.png")).getImage();
	
	private Image background = new ImageIcon(Main.class.getResource("../images/introBackground(Title).jpg")).getImage();
	private JLabel menuBar = new JLabel(new ImageIcon(Main.class.getResource("../images/menuBar.png")));
	
//기본 이미지(커서에 따라 바뀌는 이미지)
	private JButton exitButton = new JButton(exitButtonBasicImage);
	private JButton startButton = new JButton(startButtonBasicImage);
	private JButton quitButton = new JButton(quitButtonBasicImage);
	private JButton leftButton = new JButton(leftButtonBasicImage);
	private JButton rightButton = new JButton(rightButtonBasicImage);
	private JButton easyButton = new JButton(easyButtonBasicImage);
	private JButton hardButton = new JButton(hardButtonBasicImage);
	private JButton backButton = new JButton(backButtonBasicImage);
//마우스 좌표	
	private int mouseX, mouseY;					
	
//곡 선택 화면 on/off	
	private boolean isMainScreen = false;
//게임 화면	
	private boolean isGameScreen = false;
	
//트랙을 담는 배열
	ArrayList<Track> trackList = new ArrayList<Track>();
	
//곡 선택화면
	private Image titleImage;
	private Image selectedImage;
	
	private Music selectedMusic;
	private Music introMusic = new Music("introMusic.mp3", true);
	
	private int nowSelected = 0;
		
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
//시작화면 음악			
		introMusic.start();		
//곡 순서(인덱스 순서) > 변경이 용이함
		trackList.add(new Track("Parade Title Image.png", "Parade Start Image.png",
				"Parade Game Image.png", "Parade Selected.mp3", "KUWAGO - Parade.mp3"));
		
		trackList.add(new Track("Shine Title Image.png", "Shine Start Image.png",
				"Shine Game Image.png", "Shine Selected.mp3", "PIKASONIC & Couple N - Shine.mp3"));
		
		trackList.add(new Track("Ready Title Image.png", "Ready Start Image.png",
				"Ready Game Image.png", "Ready Selected.mp3", "Stessie - Ready.mp3"));
		
	
//exit버튼 (메뉴바보다 먼저 구현되어야 버튼이 메뉴바에 가려지지 않음)
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
					Thread.sleep(500);				//소리가 먼저 나오고 나중에 종료되도록
				} catch (InterruptedException ex) {
					ex.printStackTrace();
				}
				System.exit(0);						//프로그램 종료
			}
		});
		add(exitButton);
//		
//start버튼
		startButton.setBounds(500, 470, 100, 100);	//x, y, image_width, image_height
		startButton.setBorderPainted(false);		
		startButton.setContentAreaFilled(false);
		startButton.setFocusPainted(false);		
		startButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				startButton.setIcon(startButtonEnteredImage);
				startButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}
			@Override
			public void mouseExited(MouseEvent e) {
				startButton.setIcon(startButtonBasicImage);
				startButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}
			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				
				enterMain();
			}
		});
		add(startButton);
//		
//quit버튼
		quitButton.setBounds(680, 470, 100, 100); 
		quitButton.setBorderPainted(false);
		quitButton.setContentAreaFilled(false);
		quitButton.setFocusPainted(false);
		quitButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				quitButton.setIcon(quitButtonEnteredImage);
				quitButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}

			@Override
			public void mouseExited(MouseEvent e) {
				quitButton.setIcon(quitButtonBasicImage);
				quitButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				try {
					Thread.sleep(500);				
				} catch (InterruptedException ex) {
					ex.printStackTrace();
				}
				System.exit(0);	
			}
		});
		add(quitButton);
//	
//left버튼
		leftButton.setVisible(false);	//처음엔 안보이게
		leftButton.setBounds(140, 310, 60, 60);
		leftButton.setBorderPainted(false);
		leftButton.setContentAreaFilled(false);
		leftButton.setFocusPainted(false);
		leftButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				leftButton.setIcon(leftButtonEnteredImage);
				leftButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}

			@Override
			public void mouseExited(MouseEvent e) {
				leftButton.setIcon(leftButtonBasicImage);
				leftButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				selectLeft();
			}
		});
		add(leftButton);
//
//right버튼
		rightButton.setVisible(false);	//처음엔 안보이게
		rightButton.setBounds(1080, 310, 60, 60);
		rightButton.setBorderPainted(false);
		rightButton.setContentAreaFilled(false);
		rightButton.setFocusPainted(false);
		rightButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				rightButton.setIcon(rightButtonEnteredImage);
				rightButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}

			@Override
			public void mouseExited(MouseEvent e) {
				rightButton.setIcon(rightButtonBasicImage);
				rightButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				selectRight();
			}
		});
		add(rightButton);
//
//easy버튼
		easyButton.setVisible(false); // 처음엔 안보이게
		easyButton.setBounds(340, 620, 150, 50);
		easyButton.setBorderPainted(false);
		easyButton.setContentAreaFilled(false);
		easyButton.setFocusPainted(false);
		easyButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				easyButton.setIcon(easyButtonEnteredImage);
				easyButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}

			@Override
			public void mouseExited(MouseEvent e) {
				easyButton.setIcon(easyButtonBasicImage);
				easyButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				gameStart(nowSelected, "easy");
			}
		});
		add(easyButton);
//
//hard버튼
		hardButton.setVisible(false); // 처음엔 안보이게
		hardButton.setBounds(790, 620, 150, 50);
		hardButton.setBorderPainted(false);
		hardButton.setContentAreaFilled(false);
		hardButton.setFocusPainted(false);
		hardButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				hardButton.setIcon(hardButtonEnteredImage);
				hardButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}

			@Override
			public void mouseExited(MouseEvent e) {
				hardButton.setIcon(hardButtonBasicImage);
				hardButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				gameStart(nowSelected, "hard");
			}
		});
		add(hardButton);
//
//back버튼
		backButton.setVisible(false); // 처음엔 안보이게
		backButton.setBounds(1200, 50, 60, 60);
		backButton.setBorderPainted(false);
		backButton.setContentAreaFilled(false);
		backButton.setFocusPainted(false);
		backButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				backButton.setIcon(backButtonEnteredImage);
				backButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				Music buttonEnteredMusic = new Music("buttonEnteredMusic.mp3", false);
				buttonEnteredMusic.start();
			}

			@Override
			public void mouseExited(MouseEvent e) {
				backButton.setIcon(backButtonBasicImage);
				backButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				Music buttonEnteredMusic = new Music("buttonPressedMusic.mp3", false);
				buttonEnteredMusic.start();
				backMain();
			}
		});
		add(backButton);
//	
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
//		

	}
	
	public void paint(Graphics g) {
		screenImage = createImage(Main.SCREEN_WIDTH, Main.SCREEN_HEIGHT);
		screenGraphic = screenImage.getGraphics();
		screenDraw((Graphics2D) screenGraphic);			//Graphics2D로 형변환, 글자깨짐 방지
		g.drawImage(screenImage, 0, 0, null);
	}
	
	public void screenDraw(Graphics2D g) {
		g.drawImage(background, 0, 0, null);
		
		if(isMainScreen)								//곡 선택화면
		{
			g.drawImage(selectedImage, 340, 150, null);	//add가 아닌 단순 이미지
			g.drawImage(titleImage, 340, 70, null);	   	//곡 타이틀 그려주기
		}
		if(isGameScreen)								//게임화면
		{
			g.drawImage(noteRouteImage, 430, 30, null);
			g.drawImage(noteRouteImage, 534, 30, null);
			g.drawImage(noteRouteImage, 638, 30, null);
			g.drawImage(noteRouteImage, 742, 30, null);
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
			
			//타이틀 제목			
			g.setColor(Color.white);
			g.setFont(new Font("Arial", Font.BOLD, 30));
			g.drawString("KUWAGO - Parade", 20, 702);	
		}
		
		paintComponents(g);				//JLabel등을 JFrame안에 추가(add)하면 그것을 그려준다.(고정된 이미지)
		this.repaint();					//이미지가 매순간마다 계속 갱신되도록(더블 버퍼링)
	}
	
//타이틀 이미지 값을 받아서 곡 선택기능 구현	
	public void selectTrack(int nowSelected) {
		if(selectedMusic != null)
			selectedMusic.close();
		titleImage = new ImageIcon(Main.class.getResource("../images/" + trackList.get(nowSelected).getTitleImage())).getImage();
		selectedImage = new ImageIcon(Main.class.getResource("../images/" + trackList.get(nowSelected).getStartImage())).getImage();
		selectedMusic = new Music(trackList.get(nowSelected).getStartMusic(), true);
		selectedMusic.start();
	}

//첫번째 곡에서 왼쪽 눌렀을 때
	public void selectLeft() {
		if(nowSelected == 0)
			nowSelected = trackList.size() - 1;
		else 
			nowSelected--;
		selectTrack(nowSelected);
	}
//마지막 곡에서 오른쪽 눌렀을 때
	public void selectRight() {
		if(nowSelected == trackList.size()-1)
			nowSelected = 0;
		else 
			nowSelected++;
		selectTrack(nowSelected);
	}
//난이도 버튼 눌렀을 때 실행	
	public void gameStart(int nowSelected, String difficulty) {
		if(selectedMusic != null)
			selectedMusic.close();
		isMainScreen = false;
		leftButton.setVisible(false);
		rightButton.setVisible(false);
		easyButton.setVisible(false);
		hardButton.setVisible(false);
		background = new ImageIcon(Main.class.getResource("../images/" + trackList.get(nowSelected).getGameImage())).getImage();
		backButton.setVisible(true);
		
		isGameScreen = true;
	}
//뒤로가기 버튼	
	public void backMain() {
		isMainScreen = true;
		leftButton.setVisible(true);
		rightButton.setVisible(true);
		easyButton.setVisible(true);
		hardButton.setVisible(true);
		background = new ImageIcon(Main.class.getResource("../images/mainBackground.jpg")).getImage();
		backButton.setVisible(false);
		selectTrack(nowSelected);
		
		isGameScreen = false;
	}
	
	public void enterMain() {
		
		startButton.setVisible(false);//메인화면으로 넘어왔을 때 안보이도록
		quitButton.setVisible(false);	
		
		background = new ImageIcon(Main.class.getResource("../images/mainBackground.jpg")).getImage();
		isMainScreen = true;
		
		leftButton.setVisible(true);
		rightButton.setVisible(true);
		easyButton.setVisible(true);
		hardButton.setVisible(true);
		introMusic.close();
		
		selectTrack(0);//첫 시작때 첫번째 곡 선택
	}
}
```