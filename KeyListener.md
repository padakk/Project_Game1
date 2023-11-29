```java
package rhythm_game_1_11;

import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

public class KeyListener extends KeyAdapter {//키보드 이벤트
	
	@Override
	public void keyPressed(KeyEvent e) {//눌렀을 때
		if(RhythmGame1.game == null) {//게임이 진행중이지 않을 때 키보드이벤트 종료
			return;
		}
		if(e.getKeyCode() == KeyEvent.VK_S) {
			RhythmGame1.game.pressS();
		}
		else if(e.getKeyCode() == KeyEvent.VK_D) {
			RhythmGame1.game.pressD();
		}
		else if (e.getKeyCode() == KeyEvent.VK_K) {
			RhythmGame1.game.pressK();
		} 
		else if (e.getKeyCode() == KeyEvent.VK_L) {
			RhythmGame1.game.pressL();
		}
	}
	
	@Override
	public void keyReleased(KeyEvent e) {//뗐을 때
		if(RhythmGame1.game == null) {
			return;
		}
		if (e.getKeyCode() == KeyEvent.VK_S) {
			RhythmGame1.game.releaseS();
		} 
		else if (e.getKeyCode() == KeyEvent.VK_D) {
			RhythmGame1.game.releaseD();
		} 
		else if (e.getKeyCode() == KeyEvent.VK_K) {
			RhythmGame1.game.releaseK();
		} 
		else if (e.getKeyCode() == KeyEvent.VK_L) {
			RhythmGame1.game.releaseL();
		}
	}
}
```