```java
package rhythm_game_1_14;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics2D;
import java.awt.Image;
import java.awt.RenderingHints;
import java.util.ArrayList;

import javax.swing.ImageIcon;

public class Game extends Thread {//Thread > 프로그램 안에서 실행되는 작은 프로그램

//기본 이미지	
	private Image noteRouteLineImage = new ImageIcon(Main.class.getResource("../images/noteRouteLine.png")).getImage();
	private Image judgementLineImage = new ImageIcon(Main.class.getResource("../images/judgementLine.png")).getImage();
	private Image gameInfoImage = new ImageIcon(Main.class.getResource("../images/gameInfo.png")).getImage();

	private Image noteRouteSImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteDImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteKImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();
	private Image noteRouteLImage = new ImageIcon(Main.class.getResource("../images/noteRoute.png")).getImage();

	
	private String titleName; 	//현재 실행할 곡
	private String difficulty;	//난이도
	private String musicTitle;	//곡 제목
	private Music gameMusic;	//실행할 곡
	
	
//노트 에니메이션 구현(배열로 관리)
	ArrayList<Note> noteList = new ArrayList<Note>();
	
	
	public Game(String titleName, String difficulty, String musicTitle) {//생성자
		this.titleName = titleName;
		this.difficulty = difficulty;
		this.musicTitle = musicTitle;
		gameMusic = new Music(this.musicTitle, false);
		
	}
	
	
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
		
		
		for(int i=0; i<noteList.size(); i++) //노트 하나씩 그려주기
		{
			Note note = noteList.get(i);
			note.screenDraw(g);
		}
		
		
		//안티앨리어싱(글씨깨짐방지)
		g.setRenderingHint(RenderingHints.KEY_TEXT_ANTIALIASING, RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
		
		//gameInfo 글씨			
		g.setColor(Color.white);
		g.setFont(new Font("굴림", Font.BOLD, 26));
		g.drawString(titleName, 20, 702);
		g.drawString(difficulty, 1190, 702);
		
		g.setFont(new Font("Arial", Font.BOLD, 26));
		g.setColor(Color.DARK_GRAY);
		g.drawString("S", 470, 609);
		g.drawString("D", 574, 609);
		g.drawString("K", 678, 609);
		g.drawString("L", 782, 609);
			
		g.setFont(new Font("Elephant", Font.BOLD, 28));
		g.setColor(Color.white);
		g.drawString("0000000", 578, 702);
		
				
	}
//버튼 눌렀을 때 (이펙트, 효과음)
	public void pressS() {
		noteRouteSImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
//		new Music("808 CH.mp3", false).start();//효과음
	}
	public void pressD() {
		noteRouteDImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
//		new Music("808 CH.mp3", false).start();
	}
	public void pressK() {
		noteRouteKImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
//		new Music("808 CH.mp3", false).start();
	}
	public void pressL() {
		noteRouteLImage = new ImageIcon(Main.class.getResource("../images/noteRoutePressed.png")).getImage();
//		new Music("808 CH.mp3", false).start();
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
		dropNotes();
	}
	
//실행되는 곡 종료	
	public void close() {
		gameMusic.close();
		this.interrupt();
	}
//노트 애니메이션 구현, 노트 채보
	public void dropNotes() {
		Beat[] beats = null;
		if(titleName.equals("KUWAGO - Parade")) {
			int startTime = 2970 - Main.REACH_TIME * 1000;
			int gap = 240;	//노트 간격
			beats = new Beat[] {
					new Beat(startTime, "S"),
					new Beat(startTime + gap * 2, "D"),
					new Beat(startTime + gap * 4, "K"),
					new Beat(startTime + gap * 6, "L"),
			};
		}
		
		else if(titleName.equals("PIKASONIC & Couple N - Shine")) {
			int startTime = 2750 - Main.REACH_TIME * 1000;
			int gap = 166;
			beats = new Beat[] {
					new Beat(startTime, "S"),
					new Beat(startTime + gap * 2, "D"),
					new Beat(startTime + gap * 4, "K"),
					new Beat(startTime + gap * 6, "L"),
					
			};
		}
		
		else if(titleName.equals("Stessie - Ready")) {
			int startTime = 2490 - Main.REACH_TIME * 1000;
			int gap = 150;
			beats = new Beat[] {
					new Beat(startTime, "S"),
					new Beat(startTime + gap * 2, "D"),
					new Beat(startTime + gap * 4, "K"),
					new Beat(startTime + gap * 6, "L"),	
			};
		}	
//		
		int i = 0;
		gameMusic.start(); //배열의 초기화에 걸리는 시간 때문에 이곳에 위치
		
//시간에 따른 노트생성		
		while(i < beats.length && !isInterrupted()) {
			boolean dropped = false;
				
			if(beats[i].getTime() <= gameMusic.getTime()) {
				Note note = new Note(beats[i].getNoteName());
				note.start();//노트 떨어뜨리기
				noteList.add(note);
				i++;
				dropped = true;
			}
//효율적으로, 자원의 낭비를 줄여줌
			if(!dropped) {
				try {
					Thread.sleep(5);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		}
		
	}
}
```